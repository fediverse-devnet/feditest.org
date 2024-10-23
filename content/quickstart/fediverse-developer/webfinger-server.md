---
title: Run the WebFinger server tests with your application running anywhere (including localhost)
breadcrumbtitle: Run the WebFinger server tests with your application
weight: 10
---

## Quickstart for your own application

Your application can run anywhere, as long as FediTest running on your development machine
can perform HTTP GET requests on it. For example, your application can be installed on your
development machine if you simply have an alias `example.com` to `127.0.0.1` in your
`/etc/hosts`; no need to put it on a real server. You can pick any DNS name you like as
long as you also use it below.

To do this, simply follow {{% pageref "/quickstart/evaluate/" %}} and answer the questions
in a way that makes sense for your own {{% gl app %}}.

## Avoiding the terminal prompts

This is described in a HOWTO: {{% pageref "/reference/howtos/avoid-terminal-input.md" %}}

## Simplifying the command-line

If you want to simplify the `feditest run` command-line because you keep running the
same tests, use one or more of the following:

`feditest create-constellation`:
: Defines a {{% gl constellation %}} as a map of {{% gl node %}} role names to
  {{% gl node %}} definitions.

`feditest create-session-template`:
: Creates a JSON file that enumerates the tests in the sequence they will run.

`feditest create-testplan`:
: Collects all the other information on the `feditest run` command-line into a
  {{% gl TestPlan %}} that can be run with `feditest run --testplan <testplan.json>`.

## Drilling down on one failing test

This is described in a HOWTO: {{% pageref "/reference/howtos/drilling-down-on-a-failing-test.md" %}}
