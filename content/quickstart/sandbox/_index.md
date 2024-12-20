---
title: 'Understanding FediTest: the toy "sandbox" protocol'
breadcrumbtitle: Sandbox protocol
weight: 30
---

The FediTest testing framework can be used for testing any distributed system, using
any protocol, not just the ActivityPub-based Fediverse.

To illustrate how, we created an extremely trivial example protocol, the "sandbox" protocol,
that hopefully makes it easy to understand how FediTest works and how it can be applied to
other protocols.

## Getting the code

* Get FediTest:

  ```
  $ pip install feditest
  ```

* Get the tests that test the Sandbox protocol (for readability, shown on several lines):

  ```
  $ git clone --recurse-submodules \
    https://github.com/fediverse-devnet/feditest-tests-sandbox.git
  $ cd feditest-tests-sandbox
  $ git checkout v$(feditest version)
  ```

## The Sandbox protocol

This trivial toy protocol is a client-server protocol that enables a client to ask
a server to perform a very simple multiplication on its behalf. The key interfaces
are contained in
[this file](https://github.com/fediverse-devnet/feditest/blob/develop/src/feditest/protocols/sandbox/__init__.py).

### Client-Server interaction

The protocol used "in production" (well, this is a toy example, so there is no production,
but there could be), is simply this:

```
class SandboxMultServer(Node):
    def mult(self, a: float, b: float) -> float:
        ...
```
When a client invokes the `mult` method on the server, the server will calculate the product of the two
variables `a` and `b` and return the result.

From a testing perspective, we want to know:

* Does a server that implements this interface implement it correctly? Does it return
  the product of the two values, or something else? We want to know this for many
  combinations of values, including corner cases.

* Does a client that uses this interface invoke the interface correctly? For example,
  it might invoke the API with a number outside of the supported range (e.g. positive
  numbers only).

To be able to test this, we need two things:

* We need to be able to write a test script that "makes the client" invoke the server,
  and return the results to the test script. This test script then can test that
  2 * 7 indeed returns 14 and the server is doing the right thing for this and other
  values.

* We need to be able to "instrument the server" to log how the client invoked the server,
  and what values, to make sure the client is doing the right thing, and not, for
  example, dropping minus signs.

### Extra interfaces

To make this possible, extra instrumentation operations are provided on the `Nodes` in
the Sandbox protocol, specifically:

On the server {{% gl Node %}}, activate logging and return the log of invocations by
the client:

```
class SandboxMultServer(Node):
    def start_logging(self):
        ...

    def get_and_clear_log(self) -> List[SandboxLogEvent]:
       ...
```

On the client {{% gl Node %}}, make the client invoke the server with particular
arguments and return the result:

```
class SandboxMultClient(Node):
    def cause_mult(self, server: SandboxMultServer, a: float, b: float) -> float:
        ...
```

(This may be a little confusing as this operation appears to make the Sandbox "client" a
"server". It does, but only with respect to FediTest and the tests run by FediTest.
These tests need to be able "make the client do things". The client still remains only
a client with respect to the to-be-tested protocol and the Sandbox server.)

### Running Sandbox tests

Using the above Git repo, you can run the Sandbox tests against two different
implementations of the server interface -- one of which is correct, and one of which is
faulty (so you can see what happens).

The easiest way to do this is to run `make`, like this:

```
$ make -f Makefile.create
```

This will create the various {{% pageref "/reference/json-files/" %}} used by FediTest.
Then, to run the actual tests:

```
$ make -f Makefile.run
```

The generated HTML reports are in `examples/testresults/`.

(To clean up, there is also a "clean" target for both Makefiles.)

## Applying this to a real-world protocol

We suggest you follow an approach such as this:

* Identify the roles of {{% gls Node %}} that you have in your system. You may have a traditional
  client-server system, or you may have a P2P system, or a more complex architecture
  (such as a [Peer Computing Architecture](https://peercomputing.net./) as in the
  ActivityPub-based Fediverse).

* Identify the interaction patterns between the {{% gls Node %}} with the different roles.
  For example, as in case of ActivityPub, you may have some HTTPS POST interactions.

* Identify some tests cases, along the lines of 2 * 7 = 14, as above: Which Node is
  going to interact with which other Nodes, saying what? What is the expected behavior, and
  what would be faulty behavior? How can it be determined whether it was correct or
  faulty? (This might be to check a return value as in the Sandbox example, or it might
  take a database query to look whether some data was updated correctly.)

* Write down the test cases similarly to how we wrote the tests cases for the Sandbox
  protocol. You will notice that to automate that, you need "extra methods" on your
  {{% gls Node %}} similarly to how we added the "Extra interfaces" for the Sandbox
  protocol above. Some will be "control" methods: "make the system do something", and some
  will be "observe" methods: "check what happened". Sometimes the same method can do both.

* How many of those methods you want to define depends on a tradeoff between test coverage
  enabled by those methods, vs effort to create and maintain them. THat's ultimately up to
  you, and you can certainly implement them incrementally.

* And then, depending on how your {{% gls Node %}} can be deployed so they can be tested,
  implement a suitable {{% gl NodeDriver %}}. In case of the Sandbox protocol above,
  this was trivial: the implementations of the {{% gls Node %}} are simply Python code
  that is run in the same address space as FediTest. In the real world, this may be more
  complicated and you may need to implement something based on {{% gl UbosGears %}} as
  we did for Fediverse software such as Mastodon.
