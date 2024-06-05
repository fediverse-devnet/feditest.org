---
title: "FediTest first draft WebFinger results (for FediDevs meetup today)"
date: 2024-06-05
author: Johannes Ernst
authorurl: https://j12t.org/
---

For today's meetup, here are some materials.

As a reminder:

* [FediTest user stories](/assets/2024-01-30/user-stories-for-feditest.jpeg)
* [FediTest architecture](/assets/2024-04-18/feditest-architecture.png)

We can report on the first back of testing for the simplest part of this all:
WebFinger service conformance.

* [Annost-annotated RFC 7033 (WebFinger)](/assets/2024-06-05/index-annost.html):
  this is where we identified what to test.
* [Test results as test matrix](/assets/2024-06-05/webfinger-server-all-wellknown-saas-imp.testmatrix.html):
  provides an overview over 17 tests against 39 implementations.
* [Test results as test transcript](/assets/2024-06-05/webfinger-server-all-wellknown-saas-imp.sequential.html):
  more detail, one implementation at a time.

There's also:

* [Test results as experimental TAP report](/assets/2024-06-05/webfinger-server-all-wellknown-saas-imp.tap.txt):
  the TAP test format is a bit out of its depth here, we may not keep generating this. Feedback
  appreciated.
* [The FediTest-native report in JSON](/assets/2024-06-05/webfinger-server-all-wellknown-saas-imp.json):
  the other reports are generated from this one.

We are looking for feedback on everything, but particularly:

* Are results like this useful to you? How can we make them better/more useful?

* How should we report discrepancies between what the standard clearly says, what the
  standard probably means to say but isn't entirely clear about, and what is widely
  implemented? Examples:

  * `application/json` is widely used as the JRD content type, not `application/jrd+json`
    as the spec implies. Error? Not error? Hard error vs soft error?

  * Some HTTP status codes other than what the spec says. E.g. some implementations report
    422 or 400 when attempting to resolve an unknown URI scheme. How should we report that?

Best avenue for feedback: [file an issue on framework and reporting](https://github.com/fediverse-devnet/feditest/issues/new)
or [file an issue on the actual tests](https://github.com/fediverse-devnet/feditest-tests-fediverse/issues/new).
Also: [Matrix group](https://matrix.to/#/#fediverse-testing:matrix.org).

{{% box %}}
This is active work in progress, and before the first named release. Documentation on
this is site is likely inconsistent. Official release coming soon.
{{% /box %}}