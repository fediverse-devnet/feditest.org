---
title: Is FediTest just for testing ActivityPub?
---

Nope, FediTest is a general framework for testing distributed systems, in particularly those that:

* have **heterogeneous {{% gls Node %}}**, i.e. {{% gls Node %}} that run software built
  by different people, potentially with different feature sets, and using different
  tech stacks. Like the many applications of the Fediverse that nevertheless need to
  communicate.

* use **complex protocols**, like the stack that makes today's Fediverse work, which
  already includes ActivityPub, ActivityStreams, WebFinger, HTTP Signatures and more.

* with non-trivial distributed state, i.e. where just getting the (decentralized) system
  into the state necessary before a test can be run is not so simple.

This is also a reason why we keep the Fediverse tests in a different Git repository than
the FediTest framework.
