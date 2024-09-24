---
title: How do I test my application with malformed messages?
---

The bad news is that this is currently not supported out of the box.

Sorry about that, we are trying to find the resources to make it possible. (Have some?
Get in touch!)

What we need to do is create a special-purpose {{% gl app %}} that can be run as at
a {{% gl Node %}}, and that can be instructed to do various terrible things, such
as emitting malformed messages.

This {{% gl app %}} would essentially:

* Be a full implementation of the entire Fediverse protocols stack including commonly
  implemented variations. Such as, for example, being able to deal with {{% gls Node %}}
  (like Threads) that require signed messages for almost everything.
* Whose entire behavior is controllable and observable through an API on a
  very fine-grained level, such as individual data elements in JSON files.
  By way of comparison, an API such as the Mastodon API is very coarse. Even the
  ActivityPub C2S API is not sufficient.
* Can emit the same data in various variations as permitted by JSON-LD.
* Can behave invalidly (such as emitting HTTP errors when there were no errors).
* Can emit data that is malformed in various ways, from missing fields to invalid
  fields, invalid encodings, broken streams etc.

The still open issues for tests that required this are [tagged with tag
"`protocol-level`"](https://github.com/fediverse-devnet/feditest-tests-fediverse/labels/protocol-level).

It's a bit of a project, so we can't just quickly hack it together. We have the
beginnings of its with the `Imp` that's part of FediTest, but it can only perform
client-side, such as for WebFinger tests, and it does not naturally extend to also act
as a server because of the requirements for DNS hostnames and TLS certs for servers.
