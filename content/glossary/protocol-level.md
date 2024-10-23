---
title: Protocol-level Testing
summary: A test activity whose goal is to determine whether a Node implements a protocol correctly.
seealsoterm: [
    'system-level',
    'diagnostic-node',
    'node-under-test'
]
---

Unlike {{% gl system-level %}}, protocol-level testing tests that a given {{% gl Node %}}
implements a protocol specification correctly.

For example, in protocol-level testing, a test may test that:
* the entry for `outbox` in an Activity JSON file is a valid HTTPS URI,
* the HTTPS URI, when accessed with GET, produces a valid JSON file,
* which is a valid ActivityStreams Collection.

On its own, protocol-level testing often can make no assurances that a given {{% gl Node %}}
will successfully interoperate with another {{% gl Node %}}, as many protocol specifications
allow implementation variations and a given pair of {{% gls Node %}} may not support each
others' choice of variations.

