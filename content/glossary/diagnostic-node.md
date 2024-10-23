---
title: Diagnostic Node
summary: A special-purpose Node that can be instrumented to behave in a particular way and provides detailed diagnostics.
seealsoterm: [
    'Node',
    'node-under-test',
    'protocol-level',
    'imp'
]
---

For example, an Diagnostic Node may be able to emit, when instructed by a test,
an invalid message to a {{% gl node-under-test %}}, to see whether the {{% gl node-under-test %}}
correctly rejects the invalid message. It also provides more detailed reports on
what messages it receives than a regular {{% gl Node %}} would.

Currently, in FediTest, the {{% gl Imp %}} is an Diagnostic Node that can perform
a variety of WebFinger queries, including invalid ones.
