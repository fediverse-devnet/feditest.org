---
title: Why aren't you simply running tests against a single application-under-test?
---

That would indeed be much simpler. However, this approach would limit what we could test,
and we'd like to cover more scenarios than what it can do.

Consider this test: we want to test whether a user `b@b.example` can
reply to posts made by user `a@a.example`. In this approach, `b.example` might be
our application-under-test, and `a.example` would have to be provided by the test setup.
To test this:

1. the test setup needs to simulate hosting a user `a@example.com`. This includes:

   * responding the WebFinger queries (over HTTPS, because the WebFinger standard
     requires the use of HTTPS)
   * responding to Actor file queries
   * having a functioning inbox and an outbox
   * implementing HTTP signatures so the original post can be delivered to `b@b.example`.

1. the test setup needs to accept and manage a "follow" request.

1. the test setup needs to be able to send a signed "create" activity.

1. the test setup needs to be able to receive the "reply" activity.

To make it realistic, not only does the test setup require HTTPS, but it needs to have
a valid certificate, and should listen to port 443, not some non-standard port (as those
are uncommon in production.)

This is doable, but not simple.

Now consider a more complex test in which a third user `c@c.example` replies to
`b@b.example`'s reply. Or in which `a@a.example` deletes a post some time after it
has been shared widely. Or where some payload gets garbled in the transition between
`b.example` and `c.example`. This simple setup would not be able to support these types
of tests very well, and those scenarios are the ones that are most difficult for
developers to get under control, so we want to support them.

So we decided that if we want to support larger {{% gls constellation %}} of {{% gls node %}},
we might as well build things correspondingly from the get-go.
