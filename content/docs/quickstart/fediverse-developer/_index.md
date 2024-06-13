---
title: Quickstart for Fediverse developers wanting to test their own application
breadcrumbtitle: For Fediverse developers
weight: 10
---

{{% warning %}}
This is early work in progress. Things are far from complete. Don't expect things
to work and you won't be disappointed :-)
{{% /warning %}}

### Simplest setup: test Fediverse applications at wubli

Get the FediTest framework for the Fediverse:

```
% pip install feditest-fediverse
```

Get the tests that test the Fediverse:
```
% git clone --recurse-submodules https://github.com/fediverse-devnet/feditest-tests-fediverse.git
```

Get trivial tests for a trivial toy protocol in which a client invokes a server to
have it multiply two numbers. It is helpful to understand how FediTest works:

```
% git clone https://github.com/fediverse-devnet/feditest-tests-sandbox.git
```

### Install FediTest

In the `feditest` directory (first repo above), build and install the Python package
in the way you prefer. E.g.

```
% python -mvenv venv
% venv/bin/pip install .
```

(If you are on the Mac, the first command may need to be `python3` rather than `python`.)

### Run the sandbox tests

In the `feditest-tests-sandbox` directory (third repo above), run FediTest. If you
checked out the repos in sibling directories and  used the venv as shown above, the
command is:

```
% ../feditest/venv/bin/feditest
```

That will show you some of the command-line options. To run actual tests:

```
% ../feditest/venv/bin/feditest run --tap
```

This runs the tests and produces a report in [TAP format](https://node-tap.org/tap-format/).

It tests two different implementations of the sandbox protocol's server side. In the
first {{% gl Constellation %}} , the first server implementation correctly multiplies
two numbers provided to it by the client. The second {{% gl Constellation %}} uses
a different, faulty server implementation that has a bug with negative numbers. So the
report shows some passed and some failed tests, and you see what happens when FediTest
encounters a failed test.

Wondering what just happened exactly? To get more output, run with verbose flags:

```
% ../feditest/venv/bin/feditest -v -v run --tap
```

It ran the default {{% gl TestPlan %}} defined in
[``feditest-default.json``](https://github.com/fediverse-devnet/feditest-tests-sandbox/blob/develop/feditest-default.json).
You can probably figure it out by looking that JSON file and the extra output. (If not,
please [file a bug](https://github.com/fediverse-devnet/feditest/issues).)

Feel free to make a copy of `feditest-default.json`, say to `feditest-play.json`, edit
that file, and then run:

```
% ../feditest/venv/bin/feditest -v -v run --tap --testplan feditest-play.json
```

### Run some real Fediverse tests

In the `feditest-tests-fediverse` directory (second repo above), run:

```
% ../feditest/venv/bin/feditest run --tap --testplan example-testplans/gargron-mastodon-social-imp-webfinger-server.json
```

This will run some Webfinger server tests on Eugen Rochko's mastodon.social account (we
hope he doesn't mind). You can see that some tests we found by reading the WebFinger
spec don't pass.

Then try this, which is the same TestPlan but with no pre-configured test account:

```
% ../feditest/venv/bin/feditest run --tap --testplan example-testplans/saas-imp-webfinger-server.json
```

It will ask you some questions. Point it to any running Fediverse SaaS instance and
see what it reports.

Or run:

```
% ../feditest/venv/bin/feditest run --tap --testplan example-testplans/manual-saas-follow.json
```

This lets you test whether an account on some Fediverse SaaS {{% gl Node %}} and follow
another account on another {{% gl Node %}}.

But: tf you are running this, you are surely now thinking: **What terrible automation!**

Yep! This {{% gl TestPlan %}} uses {{% gls NodeDriver %}} that know nothing about the
applications they drive. So you are the {{% gl NodeDriver %}} and have to enter
everything manually. We are gradually adding automation for select applications, see
"next steps" in {{% pageref "/blog/2024-04-30-update.md" %}}).

## Run with automatic Node provisioning via UBOS

* Go to the [UBOS Developer setup](https://ubos.net/docs/development/setup/), pick
  one of the options, and follow the instructions. Ignore what it says about UBOS Mesh.

* Come back here once you have your development virtual machine running, and the Linux
  container in it.

* Now, in the non-root shell you started in your container with `machinectl shell ...`,
  create a suitable working directory and do the same as above:

  * Clone the three repositories.

  * Install Feditest.

  * Run everything described above (if you like).

But now you can also run:

```
% ../feditest/venv/bin/feditest run --tap --testplan example-testplans/ubos-mastodon-webfinger-server.json
```

(Tests Mastodon's WebFinger server support, running Mastodon locally)

```
% ../feditest/venv/bin/feditest run --tap --testplan example-testplans/ubos-wordpress-webfinger-server.json
```

(Tests WebFinger server support of WordPress with the ActivityPub plugin, running locally)

Or, you can run them both as part of the same {{% gl Testplan %}}:

```
% ../feditest/venv/bin/feditest run --tap --testplan example-testplans/ubos-mastodon-wordpress-webfinger-server.json
```

To re-iterate, those latter {{% gls TestPlan %}} instantiate the {{% gls Node %}} they need
automatically in the local Linux container before the tests run, and take them back down afterwards.
You can see how that lets us test more complex {{% gls Constellation %}} as we progress
with the project.

## Feedback?

This is all work in progress, of course, so YMMV!

Issues? Questions? Chat with us
on Matrix on [#fediverse-testing:matrix.org](https://matrix.to/#/%23fediverse-testing:matrix.org).
