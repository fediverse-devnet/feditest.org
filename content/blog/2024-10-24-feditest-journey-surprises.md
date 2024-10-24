---
title: "Developing FediTest: a journey with surprises"
date: 2024-10-24
author: Johannes Ernst
authorurl: https://j12t.org/
---

We {{% pageref "/release-notes/0.4.md" "released FediTest V0.4" %}} yesterday, which is a
significant step forward towards making FediTest more useful when testing Fediverse
implementations in the real world.
The journey has been longer and more time-consuming than we<sup>(*)</sup> expected.
I thought I outline a few reasons why.

* We first thought we could build FediTest on top of the PyUnit unit testing framework.
  It does a lot of things already that we wanted FediTest to do, like discovering tests
  and producing reports etc. We figured that we could at least use its guts and perhaps
  implement more suitable command-line arguments on top to specify FediTest-specific
  things like {{% gls testplan %}} and {{% gls constellation %}}.

  Diving into it, after a while it became clear that this was not a good idea. It would
  have been a nightmare to explain and support -- even if we had been able to find
  a good way of hooking into the PyUnit codebase, which we didn't. PyUnit does so many
  things, in so many different ways, while FediTest needs some rather unusual things (like
  running the same tests with different {{% gls constellation %}}) that are rather more
  involved to instantiate and configure than a typical test fixture. If we married them
  together, we could not see how this would produce anything that could be understood either
  by us as developers or our future users.

  So we built something entirely new from the ground up. Which is fit-for-purpose,
  doesn't carry code we don't need and can be understood. But it took its time and a
  few extra 1000 lines of code.

* Testing the public-facing WebFinger and ActivityPub JSON documents initially seemed
  straightforward: just do a few HTTP GETs and check the results. Which went fine as far
  as WebFinger went, and we released
  {{% pageref "/blog/2024-06-05-early-results-webfinger.md" "first results" %}} some
  months ago.

  But then Meta's ActivityPub implementation in Threads started becoming available,
  and it turns out they don't even serve Actor documents without authentication of the
  client through HTTP Signatures. Considering this, we thought it was very possible, even
  likely, that other commercial ActivityPub implementors would follow the same approach.
  Which we really want to be able to test with FediTest.

  To access those access-controlled JSON documents, first keys need to be exchanged, and
  that requires HTTPS requests in both directions! Which would require our FediTest script
  to not only run an HTTP server in a parallel thread -- something we had implemented
  already! -- but at a DNS-resolvable hostname with an official TLS cert. Which wasn't
  going to happen: The FediTest script is supposed to be run by a developer on their
  day-to-day workstation, with changing IP address and behind firewalls and all that.
  And requiring a tunnel with a public endpoint seemed impractical for many developers
  and many scenarios.

  If would also require FediTest to implement a significant part of the Fediverse
  protocol stack itself, which we wanted to avoid.

  So we changed the architectural assumption, with the corresponding major code changes.
  The FediTest code itself would not attempt
  to be a web server (out goes the web server code!). Instead, for all other than the most
  basic situations (WebFinger!) it would delegate to a special-purpose
  {{% gl Diagnostic-Node %}} that could run remotely from FediTest itself, at a well-known
  IP address and with an official certificate.

  Yep, it makes things more complicated, but that {{% gl Node %}} is required anyway, otherwise
  how could one test other scenarios that require HTTP signatures and official hostnames
  and TLS certificates?

* That approach also allows us to create {{% gls constellation %}} of various real-world
  Fediverse applications that include specially instrumented {{% gls Node %}} that can attempt
  to disrupt the federation from the inside, so to speak. While we don't have code for
  that yet, we have are some ideas for a "Hungarian Babelbox" node (think of it as Douglas
  Adams' [Babelfish](https://hitchhikers.fandom.com/wiki/Babel_Fish) using Monty Python's
  [Hungarian Phrasebook](https://www.youtube.com/watch?v=grA5XmBRC6g) -- hilarity ensues).
  This could be used to test resilience against (unintentional) protocol bugs and
  (intentional) attacks.

* The requirement to be able to run the same tests against a local {{% gl constellation %}}
  (e.g. with all {{% gls node %}} in a virtual machine with local IP addresses and a local
  certificate authority) and across multiple servers with
  public IP address (e.g. when an {{% gl app %}} is only available as {{% gl saas %}}) made
  framework design a bit challenging, too. But we found a way.

* Locally, cleanly installing Fediverse applications such as Mastodon for testing purposes
  seemed quite possible, because of {{% gl ubosgears %}}. What we didn't
  think of was that these applications would have active defenses against running
  them locally! They generally fall into the category of defenses against
  [server-side request forgeries](https://en.wikipedia.org/wiki/Server-side_request_forgery),
  and prevent you from having them access IP addresses from ranges that are
  [not intended to be on the public internet](https://en.wikipedia.org/wiki/IP_address#Private_addresses).

  Fortunately, we found settings and/or workarounds that didn't take too much time, but
  they are application-specific and not exactly well-documented. Another chunk of time
  spent.

* Nope, merely putting hostnames into `/etc/hosts` is not good enough for some
  applications. You actually have to resolve all your hostnames with DNS.
  Mastodon won't even let you create new accounts from the command-line unless the
  domain name of the e-mail address DNS-resolves and is fully-qualified. (Not that the
  error message would indicate that. This was found through trial and error.)
  So we needed a local DNS server.

  Fortunately, it turns out that `systemd-resolved` can be configured to serve DNS
  requests from the entries in `/etc/hosts`, so beyond having to find an approach, which
  took its time, the actual implementation was not very complicated.

* Similarly, many Fediverse applications are not willing to respond to HTTP (rather than
  HTTPS requests). The standards (like WebFinger and ActivityPub) require HTTPS anyway,
  and that requires certificate validation. So off we go, creating a local certificate
  authority. Fortunately, we had done this before and that itself didn't take much time.
  But... it needed more time and more code.

* Of course, if you create a local certificate authority, you need to insert that into
  your system's trust root, otherwise we can bet that some applications won't work because
  it doesn't have an application-level way of specifying an alternate trust root. Which
  turned out to be true.

  Ever heard of `update-ca-trust`? We hadn't either but we've put it to work.

* And it still didn't work. Turns out Python's virtual environments (venv) create a
  snapshot of the system trust store at the time of venv creation and use that every time
  you run code in the venv. So dynamically creating a new CA on every FediTest test run
  conflicted with using a venv, which was infuriating. (Why would it do this in the first
  place? Does venv have Linux container envy?)

  Workaround: All HTTPS requests made by FediTest themselves use a custom
  `request.Session` instance that points to the system trust root, not the venv trust
  root.

* And it still didn't work with WordPress. Turns out WordPress also ships its own
  certificate authority. Probably in the name of user-friendliness. Or to mess with
  people like us. Or both.

  Workaround: overwrite that file with a symlink to the system trust store upon
  deployment of WordPress.

* Speaking of user-friendliness and WordPress. Sending outgoing ActivityPub messages generally
  requires an {{% gl app %}} to queue outgoing requests separately. Mastodon runs a separate daemon
  for that. WordPress doesn't -- and instead has a pseudo-queue that is worked down
  gradually when HTTP requests come in, as a side effect of people visiting the site.
  Which is what the ActivityPub plugin for WordPress uses. Which means that in an
  automated testing environment without users, like when running FediTest, outgoing
  ActivityPub posts are never even attempted.

  Workaround: extra HTTP requests on the WordPress instance. At the time of this writing,
  they work most of the time, but not all the time. We are
  [hoping for help](https://github.com/fediverse-devnet/feditest/issues/397).

* We script Mastodon and WordPress through the Mastodon API (built into Mastodon, and
  available [as plugin](https://wordpress.org/support/plugin/enable-mastodon-apps/) for
  WordPress). Sounds great, right? Except that the Mastodon API is underdefined and
  there are significant differences in how the two code bases implement it. (Anybody
  want to write a test suite for Mastodon API implementations? Or rather, come up with
  a better API, e.g. based on the ActivityPub API? You have our vote.)

  Example: Both Mastodon and WordPress+plugins  implement a "search" API endpoint that is used
  to trigger the lookup of remote actors. In Mastodon, one can use this API to look up
  remote actors specified as `https` Actor URIs. In WordPress+plugins, only
  `acct` handles (e.g. `@user@example`) can be searched for. So if you develop against
  Mastodon first -- as we did -- and use an ActivityPub-centric (rather than WebFinger-centric)
  approach to identifiers -- as we did -- you come to realize later that this cannot be
  made to work for WordPress+plugins.

  So we had to go back and rewrite a bunch of code to be driven
  `acct` handles rather than ActivityPub `https` URIs. More work.

* How does one talk to the Mastodon API anyway?
  [mastodon.py](https://mastodonpy.readthedocs.io/en/stable/) looked like a great Python
  library for exactly that purpose, so we used it. Except that debugging the differences
  between the Mastodon and the WordPress plugin implementations one level removed
  turned out to take more time than ripping it back out and going direct.

  So that's what we did. More rewrites.

* And of course one needs an OAuth access token to access the API. Where to get it from?
  Without the user and their browser in the loop?

  Fortunately, in case of Mastodon, they still allow user/password-based token issuance
  in 4.2 (although it is supposed to go away). In case of WordPress, we implemented a
  [hack suggested](https://wordpress.org/support/topic/programmatically-obtaining-oauth-token-for-testing/#post-18025939)
  by the Enable Mastodon Apps plugin developer. Open-source developers and their
  responsiveness for the win!

* But overall, the biggest time sink has been conceptual. What are the bits and pieces,
  the objects and classes, the configuration options, test and other files, invocation
  scenarios, etc etc so that developers can:

  * as easily as possible
  * define tests that work against any Fediverse application, or combination of Fediverse
    applications in {{% gls constellation %}} that may include several {{% gls node %}}
  * tests that are rooted in standards and specifications where they exist
  * and defensible when they merely test common practice but no official specification for
    certain behaviors exists
  * on applications on which accounts need to exist that can be driven from the test
  * and driving them and other behaviors may or may not be possible through custom APIs
    for some applications
  * some of which can be installed locally, and some only exist as SaaS hosted sites
  * using any tech stack under the sun
  * and run them in as automated mode as possible
  * from a clean slate, as tests are supposed to be
  * without requiring lots of up-front integration work
  * producing results that can be understood?

  Of course, some of these requirements are in conflict. But where is the best tradeoff?
  And can we find a way of making things work with little up-front work for a new
  application, and more comfortable with more up-front work?

  I think we have found good tradeoffs. But they have not come cheaply in terms of
  time spent: The number of refactors in this project has been unusually high for these
  conceptual reasons, and I can think of a few more that could be beneficial. But ...
  at the current version, the conceptual state of affairs is reasonably good now.

Check out some of the new test reports published today: {{% pageref "/contrib/results/" %}}.

... and I certainly hope for fewer surprises in the future :-)

<sup>(*)</sup>Well, it's been mostly me making assumptions and I guess that means that's
also mostly me who got surprised!
