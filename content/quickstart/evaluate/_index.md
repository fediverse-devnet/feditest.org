---
title: Getting a taste of FediTest
weight: 10
---
## If you have just 5 minutes...

Go to {{% pageref "/contrib/results" %}} and browse some of the test results produced
with FediTest.

## If you have 10-15 minutes...

... you can run some tests yourself. This should work on macOS and most versions of Linux.

{{% include-md file="includes/install/local.md" %}}

* Run our WebFinger Server tests against a public Fediverse instance for which you have
  permission, such as your own Mastodon account. Enter the following command as shown with
  backslashes at the end of each line, or all on one line without the backslashes:

  ```
  $ feditest run \
    --filter-regex webfinger.server \
    --node client=nodes/imp.node.json \
    --node server=nodes/saas-any.node.json \
    --name 'Quickstart WebFinger' \
    --html results/quickstart-webfinger.html
  ```

  It will ask you a few questions at the console, which you need to answer. That's because
  you are testing a Fediverse instance of which FediTest knows nothing. So you need to
  tell it:

  * what hostname the {{% gl app %}} runs at that you want to test (e.g. "mastodon.example");
  * the name of the {{% gl app %}} (e.g. "Mastodon");
  * the user handle of an existing account there (e.g. "joe"); and
  * what could be a user handle of an account there but the account does not actually
    exist (e.g. "does-not-exist").

* Chances are there will be error messages on the console. That could be because there
  is something wrong with your installation, but more likely because some of the tests
  you just ran failed.

* Open the generated HTML file `results/quickstart-results.html` with a browser. It
  shows you which tests ran, and what their
  {{% pageref "/reference/outcomes.md" "outcomes" %}} were.

So what just happened? Let's look at the command line by line:

* `feditest run`: You are running FediTest sub-command `run` to run tests. There are
  other sub-commands, see {{% pageref "/reference/commands.md" %}} or run
  `feditest --help`.

* `--filter-regex webfinger.server`: Of the tests that FediTest discovers, run only
  the ones that match regular expression `webfinger.server`. (We were lazy and didn't
  bother with the `\`.) If you want to know all
  tests that FediTest discovered, run `feditest list-tests`.

* `--node client=nodes/imp.node.json` and `--node server=nodes/saas-any.node.json`:
  create a two-{{% gl node %}} {{% gl constellation %}}. The first {{% gl node %}}
  plays role `client` and is defined in file `nodes/imp.node.json`. The second
  {{% gl node %}} plays role `server` and is defined in file `nodes/saas-any.node.json`.
  The two role names (`client` and `server`) are referenced in the tests, so the tests know
  which is which: "Imp" is our WebFinger test client, and "Saas-Any" represents a generic
  Fediverse Node hosted at a public hostname.

* `--name 'Quickstart WebFinger'`: Give the report a name.

* `--html results/quickstart-webfinger.html`: in addition to reporting failures on the
  terminal, generate a report in HTML.

The files with the code for the tests you just ran are in sub-directory
`tests/webfinger/server`.

## If you have 60-90 minutes ...

... then we can do better than just one-sided WebFinger tests. Instead we will test
whether two Mastodon instances communicate with each other well enough that users
experience what they expect from social media: users can follow other users on other
instances, posts get distributed to followers, not mangled in the process, they can like
posts, boost them, reply etc.

We will use a Linux container in which to run the tests. (If you are unfamiliar with
Linux containers, Red Hat has a high-level
[explanation here](https://www.redhat.com/en/topics/containers/whats-a-linux-container).)
Our container needs to run {{% gl ubosgears %}}, which automates the installation
and configuration of the Mastodon instances (and other types of {{% gls app %}} but we
don't need them right now).

{{% include-md file="includes/install/ubos-vm.md" %}}

* Run the currently defined two-{{% gl node %}} Fediverse tests with two Mastodon
  instances and produce an HTML report:

  ```
  $ feditest -v run \
    --filter system2 \
    --node sender_node=nodes/mastodon.ubos.node.json \
    --node receiver_node=nodes/mastodon.ubos.node.json \
    --name 'Mastodon vs Mastodon' \
    --html results/mastodon-mastodon.html
  ```

  Note we added the `-v` (verbose) flag. These tests take a little while -- FediTest needs to
  install two instances of Mastodon inside your Linux container! -- and with extra output
  you see more of what is going on and not get impatient!

* In this example, you don't need to enter any info in the terminal: that's because
  FediTest completely controls the two Mastodon instances it sets up, including what
  hostnames to run them at, and the names of accounts on those instances. Unlike in the
  SaaS example above, there is no need to ask you, the user.

* Open the generated HTML report in file `results/mastodon-mastodon.html` with a browser. You can use
  Firefox that's pre-installed in your VM to open that file. It is in the same location
  in the file system as in the container. (There is no FireFox in the container; only in
  the VM.)

The files with the code for the tests you just ran are in sub-directory
`tests/system2`.

## Feeling adventurous?

What about running tests that involve three Mastodon instances?

* In the same Linux container running {{% gl ubosgears %}}, execute:

  ```
  $ feditest -v run \
    --filter system3 \
    --node node1=nodes/mastodon.ubos.node.json \
    --node node2=nodes/mastodon.ubos.node.json \
    --node node3=nodes/mastodon.ubos.node.json \
    --name 'Mastodon vs Mastodon vs Mastodon' \
    --html results/mastodon-mastodon-mastodon.html
  ```

* Open the generated HTML report in file `results/mastodon-mastodon-mastodon.html` with a browser.

The files with the code for the tests you just ran are in sub-directory
`tests/system3`.

Next: {{% pageref "../fediverse-developer.md" %}}.