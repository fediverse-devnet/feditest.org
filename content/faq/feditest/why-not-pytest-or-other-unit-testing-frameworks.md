---
title: Why didn't you build on PyTest or some other unit testing framework?
---

We started that way, but it didn't turn out to be the right tool for the job.

Consider:

* Setting up {{% gls node %}} is expensive and even when automated can take some time.
  For example, how quickly can you get a fully-configured instance of Mastodon running
  on your local test machine?
  So we want to be able to tightly control which tests run, and how often {{% gls node %}}
  need to be provisioned or reset in a given {{% gl TestRun %}}. Unit test frameworks
  don't generally provide much control over this.

* We want to run the same tests against a wide variation of {{% gl Node %}}
  {{% gls Constellation %}}. For example, we want to test whether Mastodon users can
  follow users on other Mastodon instances, but also whether they can follow them on Lemmy
  and WordPress and Threads etc.
  Just writing a few parameterized tests would quickly explode the length of a
  {{% gl TestRun %}}. Unit test frameworks don't really tend to make it easy to
  control exactly which parameterization to run against which other, and which not.

So we wanted to make {{% gls TestPlan %}} explicit and editable, so developers can
run exactly what they want to run in the exact way they want, instead of the unit
testing framework picking things for them.
