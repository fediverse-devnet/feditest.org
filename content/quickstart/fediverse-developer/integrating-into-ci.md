---
title: Integrating FediTest tests into your CI pipeline
breadcrumbtitle: Integrating FediTest into CI
weight: 50
---

(Work in progress)

## Outline:

1. Create a {{% gl testplan %}} {{% pageref "/reference/json-files.md" "JSON file" %}}
   and test your {{% gl app %}} on your development system in the {{% gls constellation %}}
   that you want to use in CI as well.

1. In your pipeline, get `feditest` through PIP. Make sure you are running the right
   Python version.

1. In your pipeline, clone the `feditest-test-fediverse` repo, and check out the right
   tag. Clone all other repos that might have tests you have defined yourself.

1. In your pipeline, execute `feditest run`. It might make sense to produce a JSON
   test transcript (option ``--json <out>``) rather than an HTML report. THe JSON test
   transcript is more detailed, easier processable, and can be converted into HTML
   any time with `feditest convert-transcript`.)
