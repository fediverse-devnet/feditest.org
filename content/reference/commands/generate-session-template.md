---
title: feditest generate-session-template
---

Similar to {{% pageref "list-tests.md" %}}, this command determines which tests are
available, potentially filters the list, and then generates a JSON file that can be
used as a {{% gl TestPlanSession %}} template, i.e. a {{% gl TestPlanSession %}} but
without the {{% gls Node %}} of the {{% gl Constellation %}} filled in yet.

For more info on invocation: `feditest generate-session-template --help`.