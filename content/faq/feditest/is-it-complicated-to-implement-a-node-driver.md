---
Title: Is it complicated to implement a NodeDriver?
---

You don't actually need to do that. You can use one of the {{% gls NodeDriver %}} that
works with any application, such as the `ManualFediverseNodeDriver` or the
`SaasFediverseNodeDriver`. While it is great that they work with any application,
they don't provide much automation: instead, you will be prompted to perform a bunch
of activities you'd much rather like to have automated.

Example: a test may require that your application "create" a "Note". Without integration
of your application, you will need to do that manually (which may be fine, depending
on what you are trying to do).

If you want (some) automation, you can implement a suitable {{% gl NodeDriver %}}
gradually and focus just on automating what is most time-consuming for you. That may
be deploy and undeploy or a {{% gl Node %}}, or it may be performing a certain
activity (like "create" a "post") or making an observation (like "is account `@@b.example`
now following `a@a.example`).
