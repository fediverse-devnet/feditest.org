---
Title: Is it complicated to implement a NodeDriver for my application?
---

You may not actually need to do that. You can use one of the {{% gls NodeDriver %}} that
work with any {{% gl app %}}, such as the `FediverseManualNodeDriver` or the
`FediverseSaasNodeDriver`.

However: while it is great that they work with any {{% gl app %}},
they don't provide much automation: instead, you will be prompted to manually perform the
activities that haven't be automated by a custom {{% gl NodeDriver %}} for your
{{% gl app %}}, such as:

* "Now create a user account on this instance", or
* "As user `example1` on this instance, make a post with the content 'Good morning'", or
* "As user `example2` on the other instance, check that 'Good morning' has arrived in your
  inbox, and if so, enter 'y"."

If this manual work doesn't work for you -- probably soon after entering the same info
half a dozen times without an automated {{% gl NodeDriver %}} for your {{% gl App %}} --
you can implement a suitable {{% gl NodeDriver %}}.

You can do this gradually and focus just on automating what is most time-consuming for you
up to and including full automation.
