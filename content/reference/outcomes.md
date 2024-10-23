---
title: Test outcomes
weight: 10
---

In testing, usually, there are about four possible outcomes:

1. The test **passed**.<br>
   Example: a calculator being tested correctly calculated 1+1=2.

1. The test **failed**.<br>
   Example: a calculator being tested calculated 1+1=3.

1. The test was **skipped**.<br>
   Example: some tests cannot run in certain configurations, such as on certain operating
   systems. The test checked and refused to run.

1. The test had an **error**.<br>
   The test itself (and not the system being tested) was faulty.

FediTest supports all of those, but we report even finer details when a test failed.

As an example, consider a WebFinger implementation. The WebFinger JSON, according to
the RFC, needs to be served with content type `application/jrd+json`. But in the wild,
many applications serve it as content type `application/json`. This is against the spec
but clearly not as much of a problem as it would be if the WebFinger query returned
a GIF image.

So FediTest distinguishes test failures in two extra dimensions:

* **Specification conformance**: does the implementation do what the protocol specification says?
  The possible values are:

  * `MUST`: The specification is clear that this is required.

  * `SHOULD`: The specification encourages this but it is not required.

  * `IMPLIED`: The specification is silent on the issue, but that may have been an
    editing mistake as it otherwise won't work.

  * `UNSPECIFIED`: We don't know whether this is required or not.

* **Interoperability impact**: what kind of impact does this aspect of the implementation have
  on successful interoperability with other software in the real world?
  The possible values are:

  * `PROBLEM`: This will likely cause interoperability problems.

  * `DEGRADED`: This will degrade but not entirely disable interoperability. For example,
    this value is used when formatted text arrives as plain text.

  * `UNAFFECTED`: This has likely no impact on interoperability.

  * `UNKNOWN`: We don't know the impact on interoperability.

When reporting test failures with FediTest, tests indicate the type of failure on these two dimensions.

In the WebFinger example above, we would report `MUST` (the spec is clear) but also
`UNAFFECTED` (it works just fine in the real world).

That way, developers can easily determine which of the reported failures are truly important
to fix and which are not and prioritize accordingly.