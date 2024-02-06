---
title: 'What do we mean when we say "Fediverse Test Suite"?'
date: 2024-02-05
author: Johannes Ernst
authorurl: https://j12t.org/
---

Let's start with the term [Test Suite](https://en.wikipedia.org/wiki/Test_suite).
This is what Wikipedia has to say about it:

> In software development, a test suite, ... is a collection of test cases that
> are intended to be used to test a software program to show that it has some
> specified set of behaviors.

About [Test Case](https://en.wikipedia.org/wiki/Test_case), it says:

> ... a test case is a specification of the inputs,
> execution conditions, testing procedure, and expected results that define a
> single test to be executed to achieve a particular software testing objective,
> such as to exercise a particular program path or to verify compliance with a
> specific requirement.

and

> Test cases underlie testing that is methodical rather than haphazard.

This latter sentence is really important. One of our goals is to make it possible
for a range of parties (more about that later) to methodically test the Fediverse,
which would be a significant advance over the state of the art.

An important aspect of testing methodically is to get as much test coverage as possible,
i.e. systematically tests as large of a percentage of the behaviors of the Fediverse
as possible. There are significant technical (and organizational, and resource) challenges
to overcome to accomplish this (more about this later), but our goal is to get that
coverage across all components as high as we can make it.

The Fediverse, for our purposes here, is as a global network of communicating "Fediverse
instances", i.e. running software applications, implemented and operated by a variety of
parties. These instances interact with each other on a voluntary basis, based on set of
agreed-upon protocols and conventions.

Today's Fediverse is centered around the
[ActivityStreams Core](https://www.w3.org/TR/activitystreams-core/),
[ActivityStreams Vocabulary](https://www.w3.org/TR/activitystreams-vocabulary/) and
[ActivityPub](https://www.w3.org/TR/activitypub/) standards
as published by the [World-Wide-Web Consortium](https://www.w3.org/). However, today's
Fediverse would not be functional with only agreement on those W3C standards.
For example, the use of the IETF's
[RFC7033: WebFinger](https://www.rfc-editor.org/rfc/rfc7033) standard is also required,
and a fairly significant number of conventions that are not standardized and sometimes
only partially codified, such as:

* The use of HTTP Signatures in a now-obsolete draft, as pioneered by Mastodon.

* Various [Fediverse Enhancement Proposals](https://codeberg.org/fediverse/fep).

* The use the "Note" object in preference to many other defined object types which are
  not broadly understood by participating software.
* Conventions for using only a subset of the power of JSON-LD.

... and more.

So our scope here is not merely ActivityPub, but the set of all protocols and conventions
that are used to make today's Fediverse work. Of course, the Fediverse is not a static
construct. Developers build new cool features all the time, which often require, or
suggest extensions or improvements to some of the protocols and conventions currently
in use. The Fediverse Test Suite is intended to be flexible in growing
with the Fediverse, in whatever direction it develops. Specifically, it should be easy
for developers to extend the test suite to support the testing of additional protocols
and conventions.

