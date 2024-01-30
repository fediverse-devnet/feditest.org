---
title: Slides shown at the W3C's Social Testing Task Force today
date: 2024-01-30
author: Johannes Ernst
authorurl: https://j12t.org/
---

The W3C's Social Community Group -- shepherd of the ActivityPub and ActivityStreams
standards -- has a Testing Task Force. It had a meeting today, and I showed these slides,
figured I put them here, too. (These are work in progress, and more notes than "presentation"
slides, my apologies for the lack of prettiness.)

First, on user stories. Like for all systems, it is useful to consider who we write
a Fediverse test suite for, and what they are trying to accomplish with it. In this
slide, we have two personas shown in separate columns, with a list of what they want to
do and why.

![User stories for FediTest](/assets/2024-01-30/user-stories-for-feditest.jpeg)

Other personas might also want to run certain tests, as was pointed out during the call
today. For example, an end user might want to determine why somebody else's Fediverse
server does not accept their content. This is out of scope for FediTest, however.

Secondly, on the question of: how do we translate the various standards documents
(like ActivityPub, or WebFinger) into the set of tests that test them? And then, how
do we make sure we have good test coverage, and how do we evolve tests with the specs
change?

I came up with something very hacky, which is to simply copy the spec HTML, and inline
test annotations right there. Here is an example. The things in the red boxes are my
test annotations into the ActivityPub standard. They are added by means of a custom
web component (`<annost-test>` at the bottom) which renders into the box with the red
border.

![Test specifications inlined into the standard](/assets/2024-01-30/test-annotations-in-spec.jpeg)

Is it a good idea? Not sure yet, and it certainly can be made better, but perhaps
worth considering. It certainly wins today's prize for hackiness :-)

(This is also worth a separate blog post in the future.)
