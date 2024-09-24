---
title: Node Driver
plural: Node Drivers
summary: An API to provision, unprovision, control and observe Nodes.
seealsoterm: [
    'Node'
]
---

Node Drivers have been implemented in various ways already, from the entirely manual
("Dear user, please now install application X somewhere and enter its DNS name") to
the automated (see {{% gl ubosserver %}}).

In the code, there are actually two abstractions:

* `NodeDriver`: knows how to provision and unprovision {{% gls Node %}}, i.e. set up
  and tear down {{% gl app %}} instances.
* `Node`: knows how to control and observe the instance of an {{% gl app %}}, such as
  through an API that can create Posts or observe that they have arrived.
