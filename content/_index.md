---
title: FediTest
breadcrumbtitle: Home
layout: front
---

{{% box "landing" %}}
{{%   box "graphic" %}}
![Illustration](/assets/front.png)
{{%   /box %}}
{{%   box "title" %}}
# FediTest
Testing distributed, heterogeneous systems...

...with complex protocols

...such as the Fediverse.
{{%   /box %}}
{{% /box %}}

{{% box "news section" %}}
## News

{{% recentposts 4 %}}
{{% /box %}}

{{% box "section" %}}
## Getting started

* If you are just **investigating FediTest** and want to get a taste for it:
  {{% pageref "/quickstart/evaluate/" "start here" "big" %}}

* If you are a **developer of a Fediverse/ActivityPub application** and want to use FediTest
  to test it: {{% pageref "/quickstart/fediverse-developer/" "start here" "big" %}}

* If you want to test **some other protocol** that not one of the protocols that comprise the
  ActivityPub-based Fediverse (WebFinger, ActivityPub, HTTP Signatures etc): coming soon.
{{% /box %}}

{{% note %}}
FediTest is at version 0.4. Given this version number, don't expect perfection 😎
But it is beginning to be useful.
{{% /note %}}

{{% box "section" %}}
## Contribute or get in touch:

* On the Fediverse: <a rel="me" href="https://mastodon.social/@feditest">@feditest@mastodon.social</a>
  <link rel="me" href="https://mastodon.social/@feditest">

* Matrix discussion group for FediTest: [#fediverse-testing:matrix.org](https://matrix.to/#/%23fediverse-testing:matrix.org)

* Code and issues on Github: [FediTest framework](https://github.com/fediverse-devnet/feditest/),
  [FediTest tests for the Fediverse](https://github.com/fediverse-devnet/feditest-tests-fediverse/).
{{% /box %}}
