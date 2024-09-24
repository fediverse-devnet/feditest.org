---
title: Test Plan Session
plural: Test Plan Sessions
summary: A sequence of tests to run with a particular Constellation during a TestRun.
seealsoterm: [
    'Constellation',
    'TestPlanSessionTemplate',
    'TestRun'
]
---

A {{% gl TestPlan %}} may contain several Test Plan Sessions. This allows FediTest to
run, for example, the same sequence of tests against multiple {{% gls Constellation %}},
e.g. to compare which combinations of servers interoperate more reliably with each other
compared to which others.