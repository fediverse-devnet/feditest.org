---
title: "Feedback from early show-and-tells"
date: 2024-03-29
author: Johannes Ernst
authorurl: https://j12t.org/
---

We did a few rounds of early show-and-tell, including:

* [this session](https://fedidevs.org/notes/2024-03-07/) in a meeting of the [Fediverse Developer Network](https://fedidevs.org/);
* [this session](https://fediforum.org/2024-03/session/5-e/) at [FediForum](https://fediforum.org/);
* and a few one-on-one sessions with potential users.

The main points of feedback were:

1. Ah, interesting, is it done yet?

   Comment: I take this as something positive :-)

2. You are doing something complicated (supporting several nodes in a server constellation). Can't you start by doing something simpler, such as simply running a few curl commands against a server in the cloud?

   Comment: the complex can address the simple (e.g. single server), but it's not viable to extend the simple to the complex (e.g. multi-server constellations) where all the bugs lurk that today, practically, aren't findable. We want to cover the entire spectrum.

3. FediTest should provide some value immediately to a new developer after a very quick installation. It can provide more value later after a more complex setup.

   Comment: that sounds smart, let's see what we can do.

4. Implement [TAP](https://testanything.org/). Or something that is like TAP that is easily machine-processable.

   Comment: Sounds like a good idea.

5. For the "test all" scenario, produce a table with application conformance similar to what is often done for web browser features, e.g. see [here](https://caniuse.com/flexbox).

   Comment: That may be a bit more complicated here, because tests tend to involve at least two application nodes, but the idea seems good.

6. I need it to test X.

   Comment: file an issue here https://github.com/fediverse-devnet/feditest-tests-fediverse/issues so we can keep track of it.

Also, nobody objected or proposed something else than my hack-up-the-spec [annost](https://github.com/jernst/annost) hack. Admittedly nobody has been particularly enthusiastic either, but people see the advantages it has for traceability. So we'll go with that for now.

Have more feedback or questions? Post with a mention to [@feditest](https://mastodon.social/@feditest).

