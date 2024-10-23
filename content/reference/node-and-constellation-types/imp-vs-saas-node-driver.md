---
title: ImpInProcessNode vs FediverseSaasNodeDriver
weight: 10
---

Purpose: {{% gl protocol-level %}}.

## About this constellation type

The `ImpInProcessNodeDriver` is the {{% gl NodeDriver %}} for a special-purpose
{{% gl diagnostic-node %}} that runs the {{% gl imp %}} and can act only as a client.
It is built into FediTest (well, it is one of the files in the default distribution) and
used as the client for {{% gl protocol-level %}} conformance of WebFinger servers.

The `FediverseSaasNodeDriver` is a {{% gl NodeDriver %}} that is entirely generic, and
can be used with any Fediverse application. It requires the user to specify:

* a resolvable DNS host name at which the application-under-test runs;
* the identifiers of various accounts that need to exist on this instance. The details
  of which accounts need to exist depend on the tests being run.

Both host name and account identifiers can be specified either interactively during
`feditest run` or as parameters in the {{% gl Node %}} definition.

## Recommended for

A constellation with two {{% gls node %}}, instantiated with `ImpInProcessNode` and
`FediverseSaasNodeDriver`, respectively, is the simplest set up for {{% gl protocol-level %}}
WebFinger server-side compliance of applications that are already hosted somewhere
public on the internet.

