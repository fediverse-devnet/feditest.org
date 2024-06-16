---
title: Constellation
summary: The set of Nodes participating in a particular Test Plan Session.
seealsoterm: [
    'Node',
    'TestPlanSession'
]
---

A Constellation's {{% gls Node %}} have role names, and one application instance is
assigned to each role name.

For example, to test whether ActivityPub actors can follow each other, a first actor on
a first {{% gl Node %}} needs to create a Follow activity, and the second actor on
the second {{% gl Node %}} needs to create an Accept activity.

If we wanted to test whether actors on Mastodon can follow Actors on WordPress, we would
assign a Mastodon instance in the first role of the Constellation, and WordPress in the
second role. If we wanted to test whether actors on WordPress can follow actors on Mastodon,
we would keep the test as-is, but assigned WordPress to the role previously played by Mastodon,
and vice versa.

With this approach, we can also test whether actors on Mastodon can follow actors on
another Mastodon instance: just assign to different Mastodon instances to the two roles.
