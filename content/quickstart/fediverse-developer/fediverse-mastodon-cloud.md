---
title: Run some ActivityPub tests with a Mastodon partner node and your application in the cloud
breadcrumbtitle: Run ActivityPub tests with cloud Mastodon
weight: 20
---

{{% warning %}}
Coming soon past version 0.2. The documentation is ahead of the code! When has that ever happened?
But here we are.
{{% /warning %}}

Other than WebFinger, for almost anything interesting to happen in the Fediverse, we need
at least two servers that can perform HTTPS operations on each other. For example, a "follow"
requires the would-be follower {{% gl Node %}} to post a "follow" activity and the
would-be-followed {{% gl Node %}} to post an "accept" activity in response. Some
implementations even require that a key exchange happens first because they only
accept requests authenticated with HTTP signatures.

This makes the FediTest setup more complicated. Here is one setup with moderate complexity:

## Pre-requisites

* Your application runs at a public DNS hostname and with an official TLS certificate.
  This could be because you have it installed on some server that's accessible for the
  internet, or you tunnel a public DNS name to localhost.

* You (perhaps temporarily) run an {{% gl ubosserver %}} that is also publicly accessible.
  Setting this up takes only a few minutes by clicking through the Amazon EC2 wizard that is
  described and linked to from [here](https://ubos.net/docs/operation/installation/x86_ec2/).

* Set up a an official DNS hostname for that {{% gl ubosserver %}} and make sure you wait
  long enough for DNS information to propagate.

## Create a TestPlan that uses your {{% gl Constellation %}}

* Copy a suitable {{% gl TestPlan %}} as a point of departure, such as:

  ```
  % cp examples/testplans/fediverse-follow-manual-saas.json myapp-follow-mastodon-ubos-ec2.json
  ```

* Find the section in the copy that defines the {{% gl Constellation %}} by looking for
  keyword `constellation`. It looks like this:

  ```
          "constellation": {
              "roles": {
                  "leader_node": {
                      "nodedriver": "saas.SaasFediverseNodeDriver",
                      "parameters": null
                  },
                  "follower_node": {
                      "nodedriver": "saas.SaasFediverseNodeDriver",
                      "parameters": null
                  }
              },
              "name": "Any Saas application vs any SaaS application"
          }
  ```

* You see that it defines two roles: `leader_node` and `follower_node`. Decide whether,
  for this test, you want an Actor in your application to follow an Actor in Mastodon
  (in which case your application would be at `follower_node` and Mastodon would be
  at `leader_node`) or vice versa.

* Now you fill in parameters for your application, as you probably did before
  {{% pageref "webfinger-server-localhost.md" "here" %}}.

* Now modify the other {{% gl Node %}} section for Mastodon which will be automatically
  provisioned on your {{% gl ubosserver %}} in the cloud. You need to change the name of
  the `nodedriver`, and set a few parameters, so it looks like this:

  ```             "follower_node": {
                      "nodedriver": "mastodon.MastodonUbosNode",
                      "parameters": {
                          "app": "Mastodon",
                          "hostname": "example.com",
                          "sshuri": "ssh://shepherd@example.com",
                          "identity_file": "id_rsa"
                      }
                  }
  ```

  * `app`: the name of the application, here Mastodon, mostly for reporting purposes.
  * `hostname`: the DNS hostname at which your Mastodon instance will run. Make sure the
    DNS record actually points to your {{% gl ubosserver %}}.
  * `sshuri`: the SSH URI used by FediTest to invoke {{% gl ubosgears %}} commands on
    your {{% gl ubosserver %}}, so it can provision (and unprovision) your Mastodon
    instance.
  * `identity_file`: optionally, if you specified a key pair for your EC2 instance that's
    not your default keypair, the name of the name of the file that contains the
    private key with which to connect.

## Now run the TestPlan

```
$ feditest run --testplan myapp-follow-mastodon-ubos-ec2.json --html myapp-follow-mastodon-ubos-ec2-results.html
```

Open `myapp-follow-mastodon-ubos-ec2-results.html` to see the results.
