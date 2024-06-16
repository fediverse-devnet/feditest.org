---
title: Getting a taste of FediTest
breadcrumbtitle: Getting a taste
weight: 10
---

## If you have 10 minutes...

{{% include-md file="includes/install/local.md" %}}

* Run our WebFinger Server tests against a public Fediverse instance for which you have
permission, such as your own Mastodon account.

  ```
  $ feditest run --testplan examples/testplans/webfinger-server-imp-vs-saas-any.json --html quickstart-results.html
  ```

  It will ask you a few questions (you are running a generic {{% gl TestPlan %}} after all).
  Make sure to enter account identifiers with an `acct:` scheme, such as `acct:myaccount@example.com`.

* Open the generated HTML file `quickstart-results.html`. You find the tests whose results
you just opened in directory `tests/webfinger/server`.


## If you have 60-90 minutes ...

{{% warning %}}
Coming soon past version 0.2. The documentation is ahead of the code! When has that ever happened?
But here we are.
{{% /warning %}}

... then we can do better than just one-sided WebFinger tests. Instead we will test
how Mastodon interacts with WordPress's ActivityPub support with brand-new instances
for both that run in a virtualized Linux container. For this Quickstart, the container
needs to run [UBOS Linux](https://ubos.net/docs/glossary/linux/), a Linux distro that's
optimized for automating installation and configuration of web applications.

{{% include-md file="includes/install/ubos-vm.md" %}}

* Run a pre-defined {{% gl TestPlan %}} that automatically installs and configures
  Mastodon and WordPress with the ActivityPub plugin, and then executes a series of tests.

  ```
  $ feditest run --testplan examples/testplans/fediverse-follow-mastodon-local-ubos-vs-wordpress-local-ubos.js --html quickstart-results.html
  ```

* Open the generated HTML file `quickstart-results.html`. You can use pre-installed
  Firefox in your VM to open that file. It is in the same location in the file system
  as in the container.

  You find the tests whose results you just opened in sub-directories of directory `tests`.

