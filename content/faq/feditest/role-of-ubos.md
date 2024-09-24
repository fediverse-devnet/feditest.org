---
title: What's the role of UBOS in Feditest?
---

[UBOS](https://ubos.net/) -- specifically UBOS Gears -- has some unique features that are
very helpful for testing Fediverse applications, such as:

* UBOS Gears knows how to install and configure complex server-side applications with a
  single command. For example, a single `ubos-admin deploy` can deploy Mastodon,
  configure the web server, set up TLS, provision a database, start the required
  background daemons, hook it all up and create an admin user etc. So setting up a
  test {{% gl constellation %}} becomes  real simple.

* UBOS knows how to restore a previously deployed application to a previously saved
  state. This is also very useful for testing. A `ubos-admin restore`, applied to
  a UBOS backup file previously created with `ubos-admin backup`, will restore
  the application to the exact same state as it had been, including database content,
  uploaded media etc. This is notoriously difficult to accomplish for server-side
  applications.

* UBOS knows how to deploy multiple instances of the same application to the same
  (virtual) host. So you can run two (or five!) Mastodon instances on the same
  machine. This is also very useful for testing if you want to run several
  {{% gls Node %}} against each other.

To use these features, you need to run [UBOS Linux](https://ubos.net/docs/glossary/linux/),
typically in a virtual machine or Linux container.

However, the use of UBOS is entirely optional. You can alternatively run FediTest on
your PC or Mac or other Linux distro without touching UBOS. Then you won't get the
UBOS management features, and you need to either manage your application
{{% gls Node %}} manually or implement something UBOS-equivalent yourself.
