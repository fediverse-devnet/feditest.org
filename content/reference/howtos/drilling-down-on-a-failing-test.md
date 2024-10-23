---
title: "How can I drill down on one specific test that's failing and not run the others?"
---

Let's say one of your tests is failing, and you want to debug what is going on.
In this case, you want to run that one test only for a while, and not run all the other
tests that are passing.

You have several options.

## Narrow down your test filter regular expression

If you invoke `feditest run` with the `--filter-regex` option, tighten your regular expression
to only match the test you are interested in. You can see what it matches if you run
with `feditest list-tests --filter-regex` with the same regular expression.

## Edit your session template

If you invoke `feditest run` with the `--session-template` option, simply delete all listed
tests except for the one you are interested in.

## Edit your test plan

If you invoke `feditest run` with the `--testplan` option, simply delete all listed
tests except for the one you are interested in.

## Interactive mode

You can run tests with the ``--interactive`` option:

```
$ fedtest run --interactive <other arguments>
```

This will cause FediTest to stop at various points and ask you how to proceed.
In particular, you can say that you want to run the previous test (that may have failed)
again.

This approach is particularly useful if it is time-consuming to set up the
{{% gl constellation %}} you need for your test.
