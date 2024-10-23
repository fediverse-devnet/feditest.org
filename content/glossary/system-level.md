---
title: System-level Testing
summary: A test activity whose goal is to determine whether a constellation of Nodes deliver the expected distributed functionality.
seealsoterm: [
    'protocol-level'
]
---

Unlike {{% gl protocol-level %}}, system-level testing ignores the involved protocols
but focuses on what the system -- all the {{% gls node %}} in the tested {{% gl constellation %}}
-- does as a whole.

For example, in system-level testing, a test may test that:
* if user A follows user B,
* and if B posts something,
* then user A will receive the post.

On its own, system-level testing makes no statement about whether or not a given {{% gl Node %}},
even if it successfully passes system-level testing, correctly implements any particular
protocol.
