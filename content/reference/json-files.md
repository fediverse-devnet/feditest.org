---
title: The various JSON files used by FediTest
---

FediTest makes use of a number of types of JSON files for different purposes. This page
attempts to explain them.

* **Node Definition**: A JSON file that defines a {{% gl Node %}}, including the
  {{% gl NodeDriver %}} and potentially a number of addition parameters, such as
  names of accounts on that {{% gl Node %}} or its hostname.

* **Constellation Definition**: A JSON file that includes one ore more {{% gl Node %}}
  definition JSON files, and arranges them into a {{% gl Constellation %}} definition
  that can be used for a {{% gl TestPlanSession %}}.

  A {{% gl Constellation %}} definition can be created from {{% gl Node %}} definition
  JSON files with `feditest create-constellation`.

* **Test Plan Session Template**: A JSON file that contains a list of tests to run,
  and information about the structure of the the {{% gl Constellation %}} required to
  run those tests. The {{% gl TestPlanSessionTemplate %}} does not yet assign
  an actual {{% gl Constellation %}} to it.

  A {{% gl TestPlanSessionTemplate %}} JSON file can be created from tests found by
  FediTest with `feditest create-session-template`.

* **Test Plan**: A JSON file that defines a sequence of {{% gls TestPlanSession %}},
  each of which is a sequence of tests to run against a particular {{% gl Constellation %}}.

  A {{% gl TestPlan %}} JSON file can be created from a {{% gl TestPlanSessionTemplate %}}
  JSON file and one more more {{% gl Constellation %}} JSON files with
  `feditest create-testplan`.

* **Test Run Transcript**: A JSON file that contains the results of a {{% gl TestRun %}},
  such as which tests were run and the result of each tests. The {{% gl TestRun %}}
  transcript also contains the {{% gl TestPlan %}} that was run and some metadata, such
  when the run was performed.

  A {{% gl TestRun %}} transcript is produced by `feditest run`.

* **Reports** can be produced in various formats with `feditest convert-transcript` from
  any {{% gl TestRun %}} Transcript.

This flow is shown graphically below:

{{% img style="text-align: center" src="/assets/reference/json-data-flow.png" alt="JSON data flow" %}}

