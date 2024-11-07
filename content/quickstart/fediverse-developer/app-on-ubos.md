---
title: Run Fediverse system tests with your application in a UBOS container
breadcrumbtitle: Run system tests with your application on UBOS
weight: 40
---

Here is an outline of the steps you need to take so your own Fediverse {{% gl app %}}
can be tested like Mastodon is in the system test setups described in
{{% pageref "/quickstart/evaluate.md" %}}. Just like there, this allows you to
run your own {{% gl app %}} against itself, but also against other {{% gls app %}}
like Mastodon or WordPress with the plugins.

## Step 1: Package your app for UBOS

This involves creating an Arch Linux package for it with extra metadata,
and template files or scripts that can parameterize the configuration so that
{{% gl ubosgears %}} can successfully deploy your {{% gl app %}} at any hostname.

The [ubos.net website](https://ubos.net/) has [tutorials](https://ubos.net/docs/gears/developer/)
for how to do that.

As a side effect, your {{% gl app %}} can now also be installed, uninstalled, backed up,
and restored with a single {{% gl ubosgears command %}} each, which may be an
attractive feature anyway independent of FediTest.

## Step 2: Create a NodeDriver implementation class for your app

When you specify {{% gls node %}} in test {{% gls constellation %}}, you specify a
{{% gl NodeDriver %}} like this:

```
{
    "nodedriver" : "MastodonUbosNodeDriver"
}
```

This name refers to a Python class that implements the FediTest `NodeDriver` abstract class
(in `src/feditest/nodedrivers/__init__.py`). This `NodeDriver` abstract class defines
a handful of methods that need to implemented for your {{% gl app %}} so FediTest
can get an instance of your {{% gl app %}} up and running, and also take it back down.

As an example, you can use the `MastodonUbosNodeDriver` (which is found in
`src/feditest/nodedrivers/mastodon/ubos.py`). Implement something similar that
works for your {{% gl app %}}.

## Step 3: Create a Node implementation class for your app

The `NodeDriver` class wants to instantiate subclass of `Node` in its `_instantiate_ubos_node()`
method. You need to implement a subclass of `Node` for your {{% gl app %}}, too.

Class `Node` has many (abstract) subclasses, each of which provides methods that allow
FediTest scripts to make the `Node` do something, or report back an observation. There is
one such subclass per protocol. If, for example, you want your `Node` to be tested as a
WebFinger server, you implement `Node` subclass `WebFingerServer`. If it is a
full-fledged Fediverse {{% gl node %}}, you implement subclass `FediverseNode` (that inherits
from `WebFingerServer`), as class `MastodonNode` does.

This can be a little involved, because there are many things tests want to test.
If your {{% gl app %}} implements the Mastodon API, you are in relative luck, because
you can simply reuse the `NodeWithMastodonAPI` class, as we do in the support
for WordPress (see class `WordPressPlusPluginsUbosNode`).


