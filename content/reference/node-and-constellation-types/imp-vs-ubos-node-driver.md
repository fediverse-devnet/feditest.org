---
title: ImpInProcessNode vs a UbosNodeDriver
weight: 20
---

Purpose: {{% gl protocol-level %}}.

## About this constellation type

The `ImpInProcessNodeDriver` is the {{% gl NodeDriver %}} for a special-purpose
{{% gl diagnostic-node %}} that runs the {{% gl imp %}} and can act only as a client.
It is built into FediTest (well, it is one of the files in the default distribution) and
used as the client for {{% gl protocol-level %}} conformance of WebFinger servers.

For more info about UbosNodeDrivers, see description in {{% pageref ubos-node-drivers.md %}}.

## Recommended for

{{% gl protocol-level %}} WebFinger tests of applications that aren't deployed publicly in
the cloud yet and for which a UbosNodeDriver exists.
