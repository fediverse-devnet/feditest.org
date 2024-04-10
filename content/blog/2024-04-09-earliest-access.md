---
title: "Early(est) access to work in progress on FediTest and the Fediverse test suite"
date: 2024-04-09
author: Johannes Ernst
authorurl: https://j12t.org/
---

If you are interested in running basically the same thing that was shown in
the recent [FediTest show and tell](https://fedidevs.org/notes/2024-03-07/), here is how you
do that.

There are many disclaimers, as this is early work.

* It doesn't really test much. The focus of this early(est) release is to let you play
  with the approach we are taking, so we can get feedback as early as possible and
  build something that might actually be useful :-)

* For now, you have to run it exactly in the configuration documented below.

* Reach out with problems at [@feditest@mastodon.social](https://mastodon.social/@feditest)
  or file [isses against the FediTest framework](https://github.com/fediverse-devnet/feditest/issues/)
  or [issues against the tests themselves](https://github.com/fediverse-devnet/feditest-tests-fediverse/issues/).

So here is what you do. It probably sounds fairly complicated, but it isn't as bad as
it sounds.

* For this early access, you need to run FediTest in a Linux container that runs
  [UBOS](https://ubos.net/).

  The easiest way to do this is to run a UBOS development virtual machine, because the
  container setup is automated there. Which virtual machine to run depends on which operating
  system and CPU architecture you are on (e.g. VirtualBox or not).

  Go to the [UBOS Developer setup](https://ubos.net/docs/development/setup/), pick
  one of the options, and follow the instructions. Ignore what it says about UBOS Mesh.

* Come back here once you have your development virtual machine running, and the Linux
  container in it.

* Now, in the non-root shell you started in your container with `machinectl shell ...`,
  create a suitable working directory and check out both the FediTest framework code
  and what wants to become the test suite:

  ```
  % git clone https://github.com/fediverse-devnet/feditest.git
  % git clone https://github.com/fediverse-devnet/feditest-tests-fediverse.git
  ```

* Build and install the FediTest framework in a virtual Python environment:

  ```
  % cd feditest
  % python -mvenv venv
  % venv/bin/pip install .
  ```

* Now change directories into the repo that contain the test and run `feditest` from
  there:

  ```
  % cd ../feditest-tests-fediverse
  % ../feditest/venv/bin/feditest
  ```

  (You may want to create an alias to make the invocation simpler, e.g.
  `alias feditest=../feditest/venv/bin/feditest` and then simply say `feditest`
  instead of the full path.)

And now you can try it out:

* See the sub-commands and flags defined so far:

  ```
  % ../feditest/venv/bin/feditest --help
  ```

* List the tests that have been defined so far:

  ```
  % ../feditest/venv/bin/feditest list-tests
  ```

* Find out more information about a particular test:

  ```
  % ../feditest/venv/bin/feditest info --test sandbox.test_1
  ```

* Tests are grouped into test sets, so they are more easily manageable. To find out
  what test sets there are:

  ```
  % ../feditest/venv/bin/feditest list-testsets
  ```

* FediTest interacts with applications-under-test by means of an abstraction called
  a `NodeDriver`. We'll probably end up with various `NodeDrivers` for applications
  that are installed/provisioned in various ways. To see what `NodeDrivers` are
  currently available:

  ```
  % ../feditest/venv/bin/feditest list-nodedrivers
  ```

and finally, the thing you have been waiting for:

* To run the default test plan at `feditest-default.json` with verbose mode turned on:

  ```
  % ../feditest/venv/bin/feditest -v run
  ```

  During the test run with this test plan, both Mastodon and WordPress with the ActivityPub
  plugin arespun up
  and torn down after the test by way of `ubos-admin deploy` and `ubos-admin undeploy`
  (see https://ubos.net/docs/operation/ubos-admin/#ubos-admin-deploy ... the Site JSON
  file referenced there is in directory `ubos-sites` in case you are curious).

* To run a non-default test plan:

  ```
  % ../feditest/venv/bin/feditest -v run --testplan example-testplans/feditest-sandbox.json
  ```

For feedback, one more time: Reach out with problems at [@feditest@mastodon.social](https://mastodon.social/@feditest)
or file [isses against the FediTest framework](https://github.com/fediverse-devnet/feditest/issues/)
or [issues against the tests themselves](https://github.com/fediverse-devnet/feditest-tests-fediverse/issues/).
