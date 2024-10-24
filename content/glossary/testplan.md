---
title: Test Plan
plural: Test Plans
summary: A sequence of tests in a Test Plan Session Template, run once each for one or more Constellations.
seealsoterm: [
    'Constellation',
    'Report',
    'TestPlanSessionTemplate',
    'TestRun'
]
---

For example, the Test Plan may have:

* a {{% gl TestPlanSessionTemplate %}} with tests that test "Follow", "Like" and "Reply",
* two {{% gls Constellation %}}, one of which runs two Mastodon instances against each
  other, and the other runs Mastodon against WordPress + plugins.

This Test Plan is executed as a unit, and the produced {{% gl report %}} can now
produce a test matrix that compares how these two {{% gls constellation %}} pass or
fail the same tests.

You run at TestPlan defined in a file with a command such as
`feditest run --testplan <testplan-file>`.