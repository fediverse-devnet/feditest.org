---
title: Constellations created with UbosNodeDrivers
weight: 30
---

Purpose (currently): {{% gl system-level %}}.

## About this constellation type

Currently, FediTest contains two `UbosNodeDrivers`:

* `MastodonUbosNodeDriver` knows how to:
  * provision (i.e. install and fully configure) an instance of Mastodon;
  * use the Mastodon API to control and observe the Mastodon instance;
  * provision new user accounts on the Mastodon instance.
* `WordPressPlusPluginsUbosNodeDriver` knows how to:
  * provision (i.e. install and fully configure) an instance of WordPress with
    several WordPress plugins, including the
    [ActivityPub plugin](https://wordpress.org/plugins/activitypub/) and the
    [Enable Mastodon Apps plugin](https://wordpress.org/plugins/enable-mastodon-apps/),
    which implements parts of the Mastodon API on top of WordPress;
  * use that WordPress implementation of the Mastodon API to control and observe the
    WordPress instance;
  * provision new user accounts on the WordPress instance.

We may implement `UbosNodeDrivers` for additional applications in the future. Also,
developers can do that on their own by using the above {{% gls NodeDriver %}} as examples.

`UbosNodeDrivers` require {{% gl ubosgears %}}, otherwise this level of automation
cannot be delivered.

## Recommended for

* Fully-automated {{% gl system-level %}}.
* Testing that requires a fresh installation of a {{% gl Node %}} without old data.
* {{% gls constellation %}} that involve more than one application (e.g. testing Mastodon against
  WordPress+Plugins).
* {{% gls constellation %}} that involve more than two {{% gls Node %}} (e.g. testing that a comment
  on a comment on a post gets delivered to the original poster).
* Fully-automated {{% gl protocol-level %}} that require the {{% gl node-under-test %}}
  and the {{% gls diagnostic-node %}} to be able to be servers at well-defined
  DNS hostnames with trusted TLS certificates.

