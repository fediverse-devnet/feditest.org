---
title: FediTest session at FediForum
date: 2024-09-13
author: Johannes Ernst
authorurl: https://j12t.org/
---

{{% slide-in-img-right src="/assets/2024-09-13/fediforum-color.png" href="https://fediforum.org/" %}}

[FediForum](https://fediforum.org/) is an unconference "for the people who move the
Fediverse forward". It took place for the fourth time September 12-14, 2024, online.
 (Disclaimer: I'm an organizer.)

Naturally, I ran a session on FediTest there. We had a number of Fediverse developers
there, including [Ryan Barrett](https://snarfed.org) and
[Jesse Karmani](https://github.com/jesseplusplus). (There were others but following
FediForum conventions, I don't want to publish their names as they didn't add their
names to the public attendee list of the session.)

I used a few slides from my {{% pageref "2024-08-08-feditest-at-dwebcamp.md" "most recent deck" %}}
and gave a brief demo -- although it reported a few more errors than it should have because
I had been out with the flu the previous week and didn't manage to get it fixed beforehand.

But the demo was interesting anyway:

* Run a single command: `feditest run --testplan <some-file.json>`, which:

* automatically spun up two instances of Mastodon;

* with auto-provisioned, local hostnames;

* with a local certificate authority (CA) and TLS certs from that local CA;

* obtained one account each on each Mastodon instance;

* used the Mastodon API to have the second Mastodon account follow the first;

* used the Mastodon API to have the first Mastodon account post something;

* and test that the second account received that post in their inbox.

Progress!
