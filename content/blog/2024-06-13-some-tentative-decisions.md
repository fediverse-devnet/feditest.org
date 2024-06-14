---
title: Some tentative decisions about FediTest
date: 2024-06-13
author: Johannes Ernst
authorurl: https://j12t.org/
---

As FediTest progresses, some questions are emerging, and the right answers aren't entirely
obvious. In this post, I list some of the questions and attempt to answer them. This is
a draft, and everything is very tentative: we are looking for feedback and are happy
to decide otherwise if somebody's reasoning is better than our own.

### Will the FediTest project regularly run tests and publish the results?

We have no current plans to do so.

**Reasoning:** The percentage of tests that we can fully automate is small as a fraction of
all interesting tests. We can automate some of it. For example, we can automate WebFinger
server tests as we have already, and we can use the
Mastodon client API to "script" certain actions for a Mastodon instance to take, like
follow another account or make a post. Many of the so far about 100 Fediverse
applications we have identified do not have an equivalent API. Even if they did, we cannot
possibly afford to build adaptors to each of those and keep them maintained.

If much testing cannot be automated, it needs to be manual. Fortunately, the FediTest
framework is built in a way that most tests can, without change, execute either in an
automated or manual fashion. Unfortunately, manual testing is a very labor-intensive
(i.e. expensive) and repetitive (i.e. boring) process. We do not have the available
resources to do so. If there were significant funding, perhaps we could find a way, but
so far no such funding is even on the horizon. (Have some? Get in touch!)

### So how does FediTest produce output that is useful?

Instead, we focus on making it possible (easy! as well as we can!) for developers to run
the tests they want to run themselves. And publish the results if they are so inclined,
perhaps even on the [feditest.org](https://feditest.org/) website.

Some of us on the FediTest project will likely do that, but there isn't a commitment
by the FediTest project itself to do this, in particular on a particular schedule.

### Which Fediverse instances will be tested, using which accounts? Do you have their permission?

We prefer to test locally installed Fediverse instances, followed by instances and accounts
volunteered by the respective application developers.

**Reasoning:** locally installed Fediverse instances do not cost somebody (who is not the
tester) any resources, and they are great from a test isolation perspective. Debugging
why a test failed is also much simpler.

This is why we already have
[UbosNodeDriver](https://github.com/fediverse-devnet/feditest/blob/develop/src/feditest/ubos/__init__.py),
which uses [UBOS Gears](https://ubos.net/docs/development/reference-gears/understanding/deploy/)
to automate the installation/provisioning and configuration of Fediverse applications such as
Mastodon and WordPress+AP on a local (usually virtualized) device. (We welcome other
implementations of the `NodeDriver` abstraction.)

But not everything can be installed locally. For example, some Fediverse software only
exists as SaaS. Other software installations require extra work by the tester (if manual
installation) or provisioning automation, like when newly packaging an application as a
[UBOS app](https://ubos.net/docs/glossary/app/). (We welcome more Fediverse software as
UBOS apps, here is [documentation](https://ubos.net/docs/development/tutorials-gears/toyapps/)
for how to package it.)

So when local installation is not possible, we use hosted instances
and accounts that their administrators have volunteered.

If we cannot automate the local installation and no suitable SaaS instance has been
volunteered, we do not include the respective application in FediTest setup.

### So I can't use FediTest to test interoperability of my code with some other application?

We are only saying that we won't have a predefined configuration for you. If you find
an instance that you can use, we'll give you instructions how to use FediTest with that
instance.
