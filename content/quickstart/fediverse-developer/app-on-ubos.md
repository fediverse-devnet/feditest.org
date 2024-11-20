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

The essence of the packaging is the creation of these two files, as in this example of a
[minimal web application](https://gitlab.com/ubos/ubos-toyapps/-/tree/main/gladiwashere-php-mysql)
that uses a MySQL database:

* `PKGBUILD`: package build file for the package (bash, from Arch Linux)
* `ubos-manifest.json`: declares the files, web server configuration, database and the
  like that the application needs.

If your {{% gl app %}} is already available in a Docker container, these files may be even
simpler. (We'll write documentation for how to do this once we have a real-world example
for this configuration.)

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

As we are using {{% gl ubosgears %}}, you can subclass `UbosNodeDriver` which provides
a lot of the required code already.  As an example, you can use the `MastodonUbosNodeDriver`
(which is found in `src/feditest/nodedrivers/mastodon/ubos.py`). Implement something similar
that works for your {{% gl app %}}.

## Step 3: Create a Node implementation class for your app

The `NodeDriver` class wants to instantiate a subclass of `Node` in its `_instantiate_ubos_node()`
method. You need to implement a subclass of `Node` for your {{% gl app %}}, too.

Class `Node` has many (abstract) subclasses, each of which provides methods that allow
FediTest scripts to make the `Node` do something, or report back an observation. There is
one such subclass per protocol. If, for example, you want your `Node` to be tested as a
WebFinger server, you implement `Node` subclass `WebFingerServer`. If it is a
full-fledged Fediverse `Node`, you implement subclass `FediverseNode` (that inherits
from `WebFingerServer`), as class `MastodonNode` does.

This can be a little involved, because there are many things tests want to test.
If your {{% gl app %}} implements the Mastodon API, you are in relative luck, because
you can simply reuse the `NodeWithMastodonAPI` class, as we do in the support
for WordPress (see class `WordPressPlusPluginsUbosNode`).

If you have some other API to your {{% gl app %}}, you can use that. If not, in the future
we are thinking of creating a JSON-RPC (client-side) implementation of `FediverseNode`:
that would make it possible for you to add a single new route to your web {{% gl app %}},
from which you can invoke the internals of your {{% gl app %}}.
([Let us know](https://github.com/fediverse-devnet/feditest/issues/215)
what you think of that idea.)

However, you can implement as many or as few methods in `FediverseNode` as you like. If
you don't implement them all, this means that either you can't run tests that require
an implementation of that method, or you need to manually perform the action during the
test run.

For example, if you were to not implement method `like_object()` in `FediverseNode`, a
test cannot automatically make your {{% gl app %}} "like" a "Note", and FediTest would skip
those tests that try to do that: the current default implementation of `like_object()` raises
a `NotImplementedByNodeError`, which causes the skip. Alternatively, you could
implement this method simply by asking the user to perform the action manually with
their web browser, and hit return when done. The `FallbackFediverseNode` implementation
of `FediverseNode` does this for most methods.

In fact, you might want to start your implementation of `Node` by simply
returning an instance of `FallbackFediverseNode` from your `NodeDriver`:
that way, you can see whether the setup works and FediTest runs, even if you have to
initially perform test actions and observations manually. Over time, you can then
implement automated versions of the same methods.
