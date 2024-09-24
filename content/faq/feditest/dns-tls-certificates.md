---
title: How do I set up DNS and TLS certificates for my to-be-tested Nodes?
---

As with so many things with FediTest, you have choices:

* You bring your own.
* You let FediTest and UBOS take care of it.

## Choice 1: you bring your own DNS setup and TLS certificates

Use this setup if the {{% gls node %}} you want to run against each other are already
running at some public hostname, with official TLS certificates etc. This could be, for
example, because you are running them in the public cloud, or with some kind of reverse
proxy setup behind your firewall.

In this case, simply specify the respective values for the `hostname` parameter in your
{{% gl Node %}} definition (see
{{% pageref "/reference/json-files/node/" %}}) in your {{% gl TestPlan %}}.

This also works if you are on a local network whose DNS server has local records, and
you issue your own TLS certificates from a local-network-wide certificate authority of
your own. Some enterprises are that way, and, probably, a bunch of hackers on their
home networks.

## Choice 2: you let FediTest and UBOS take care of it

This only works if you run the {{% gls Node %}} on a {{% gl ubosserver %}}.

If you set `hostname` parameters for your {{% gl Node %}} definitions in your
{{% gl TestPlan %}}, UBOS will automatically set up local DNS resolution for them, so
you don't have to worry about it. (It uses `systemd-resolved`'s stub resolver that
serves `/etc/hosts` content that is written by UBOS upon deploy.)

If you do not set `hostname` parameters in your {{% gl TestPlan %}},
FediTest + UBOS will allocate a valid hostname
for each {{% gl Node %}}, and automatically set up local DNS resolution for them. The
hostname is derived from the name of the {{% gl app %}} at the {{% gl Node %}}, in
an automatically created DNS domain that is either also automatically created, or
that you can specify with the ``--domain`` parameter on ``feditest run``.

If you provide TLS keys and certificate chain for your {{% gl Node %}} in your
{{% gl TestPlan %}} through {{% gl Node %}} parameters (that
would be unusual), FediTest will use that when setting up the {{% gl Node %}}. If not,
FediTest will create a temporary, local certificate authority, and use it to create new
certificates for the {{% gls Node %}} in your {{% gl TestPlan %}}. FediTest + UBOS will
make sure that these certificates are part of your system-wide trust store, so the
{{% gls Node %}} you are running trust them, and also make sure that they are cleaned
up after the {{% gl TestRun %}} is over.

Note: all automatically created DNS records, certificate authorities and TLS certificates
are **local to your FediTest / UBOS environment**, and do not interact with the world outside
of your FediTest setup.
