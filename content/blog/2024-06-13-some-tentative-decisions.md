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
all interesting tests. We already automate some of them. For example, we can automate
WebFinger server tests as we have already, and we can use the
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

### So how do I use FediTest to test interoperability of my code with some other application?

It's up to you to find an instance of that other application that you are able (and
permitted to) test against. Once you do -- whether that instance is locally installed
or somewhere public in the cloud -- you
{{% pageref "/reference/json-files/node.md" "define a Node" %}} in your {{% gl TestPlan %}}
that points to that instance and FediTest will run against it.

**Updated** October 2024.
