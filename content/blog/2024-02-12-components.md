---
title: Some thoughts on the components needed for the Fediverse Test Suite
date: 2024-02-12
author: Johannes Ernst
authorurl: https://j12t.org/
---

To create the Fediverse Test Suite, we need a number of components:

## 1. The actual tests

For example, we want to test that:

* An actor file has the right format.

* An actor can follow another actor on another server, and their "following" and "follower"
  collections are updated accordingly.

* The content of a "note" post does not get mangled when transmitted from one server to
  another server running different software.

Each of those needs to have code that exercises the server(s) under test, and observes
that the server(s) under test do the right thing. So we end up with a long list of
executable tests which interact with the server(s) under test.

Some of those tests can simply interact with a single server. For example, to check
that an actor file has the right format, all the test needs to do is know the URL of an
actor on a server, perform an HTTP GET, and then parse and check the resulting file.

Some are more complex. For example, to check that actor A on server example-a.com
can follow actor B on server example-b.com, two ActivityPub messages need to be sent
(a "Follow" in one direction, and an "Accept" in the other), and the followers and
following collections need to be checked prior and after. Depending on the setup,
in addition, WebFinger lookups may need to be performed first to determine the actor URLs.

There are many challenges in defining such executable tests, including:

* Where do the tests come from? How do they relate to standards documents? What about
  tests for behaviors that are commonly implemented in the Fediverse today, but aren't
  codified in a standard? How do we avoid arguments about which is a valid test and
  which is not?

* How do we maintain and evolve the tests, given that the Fediverse is not static and developers
  innovate all the time?

* How are the tests organized? Just one flat list probably is unmanageable.

* Some tests probably only apply to some applications, e.g. applications that implement
  certain FEPs vs those that don't?

* How does one write such a test, in particular if it involves multiple running servers
  interacting with each other?

* Can one write these tests in a way that is independent of the server-side software
  being tested? For example, it should be possible to write a test "A can follow B"
  that is independent of what software hosts actors A and B.

* Setting up servers is expensive, even if it can be automated. How can we accomplish
  test isolation while not spending most computation (or manual operator) time on server
  setup or teardown?

## 2. The test suite framework

What exactly runs those tests? There needs to be a test framework that makes it
easy to define those tests, and run them in a variety of different ways. The framework
needs to be able:

* To run tests against single servers, but also against constellations of servers running
  the same, or different software (e.g. to test the "follow" example above, and there
  are even more complex examples).

* Set up and tear down servers running specific software. What exactly this entails
  is highly dependent on the software running on a server, but may include provisioning
  (relational and other) databases, caching servers, other daemons (e.g. for message
  delivery), virtualization, TLS certificates etc.

* To repeatably set the total state of the distributed system, consisting of potentially several
  servers running different applications, to the point where the preconditions of a
  given test are met. For example, a test testing that "unfollow" works requires the
  involved servers to have an existing "follow" relationship between actors A and B,
  which in itself is a multi-step process to set up.

* To run the same test against different constellations of servers. For example, the
  test for "A can follow B" should be run as "A on Mastodon can
  follow B on (a different instance of) Mastodon", "A on Pixelfed can follow B on
  Mastodon", "A on Mastodon can follow B on Pixelfed" and "A on Pixelfed can follow
  B on (a different instance of) Pixelfed". And this list is only for two applications;
  the Fediverse has dozens of them.

* To give the tester/developer a variety of running tests, e.g. to focus down on
  a small subset of tests in a particular constellation of applications, if, say,
  some test passes all combinations of applications running on example-a.com and
  example-b.com except one.

## 3. Application integration

Before a test can be run, the test suite framework must setup the server(s) needed for
the test, by deploying and configuring the right server application(s) on it/them. Then,
while the test is being run, the test needs to be able:

* To control a server-under-test by making it do something, such as "now I want you to
  try and follow actor B with actor A", or "now I want you to collect incoming HTTP
  requests so I can get them from you later for checking purposes".

* To observe a server-under-test by checking that it has arrived in a particular state,
  such as "check that A is indeed now following B". Some observations may be fairly
  straightforward, such as by HTTP GET'ing public data from public endpoints. Others,
  such as for private messages, may not be as obvious.

Not only are there no standardized interfaces in the Fediverse for these controllability
and observability needs today, but in most cases, no interfaces at all. So how can the
test suite interact with the to-be-tested applications?

## 4. Baseline protocol implementation

If we test a given application-under-test, what is the "other side" this
application-under-test will actually talk to? If it is part of the test framework
or parts of the tests, what guarantees that the test suite's own implementation of
relevant standards (like WebFinger and ActivityPub), needed to interact with the
application-under-test is actually correct?

How do we test the tester? Are we creating an "N+1 problem"? The rationale for the
Fediverse test suite is to test the N implementations of the Fediverse protocol stack
that exist today. Do we make things better or worse by adding another, even if it is
part of the test suite?

Also, the "other side" of the application-under-test must be very flexible and
instrumentable, so a test can, for example:

* issue an invalid or incomplete request to the application-under-test, and check
  its response;
* collect detailed request-response information and use this for logging and error
  reporting, so developers can easily understand what went wrong.

--

There are the major components needed for the Fediverse Test Suite. None is particularly
simple &#x1F601;. For the purposes of this post, we just want to communicate those. We'll
have more to say about the issues and how we address them in the future.

