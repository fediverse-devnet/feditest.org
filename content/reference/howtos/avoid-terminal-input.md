---
title: "Q: How can I avoid interactive terminal input when running tests?"
---

In {{% pageref "/quickstart/evaluate.md" %}}, you had to enter the hostname of the
{{% gl node %}} where your to-be-tested {{% gl app %}} runs, and the user handles of
an account and a non-existing account. That works fine for a one-time evaluation, but
is cumbersome when one needs to do it frequently.

Fortunately, you can set this up once and don't need to do it again. Do this:

* Copy the `nodes/saas-any.node.json` (say to `mynode.json`) and run FediTest with
  that file as the `server` {{% gl node %}} instead of `nodes/saas-any.node.json`.

* Every time FediTest asks you for something, it will also tell you how it internally
  refers to the value it asks for. Write that down, and after the test run is finished,
  enter those values into your `mynode.json`. Then you will never have to enter them
  at the command-line instead.

For example, your `mynode.json` initially looks like this:

```
{
    "nodedriver" : "FediverseSaasNodeDriver"
}
```

Then FediTest might ask you a question such as:

> TESTER ACTION REQUIRED: Enter the hostname for the Node of constellation role "server" (node parameter "hostname")

Note it says the "node parameter" is called "hostname". This tells you that you can add the
value to your `mynode.json` as a parameter. It is similar for the other questions.

Your file, at the end, may look like this:

```
{
    "nodedriver" : "FediverseSaasNodeDriver",
    "parameters" : {
        "hostname" : "mastodon.example",
        "app" : "Mastodon"
    },
    "accounts" : [
        {
            "role" : null,
            "account_userid" : "joe"
        }
    ],
    "non_existing_accounts" : [
        {
            "role" : null,
            "not_existing_account_userid" : "does-not-exist"
        }
    ]
}
```

Which parameters can be specified, and how to specify accounts and non-existing accounts
depends on the {{% gl NodeDriver %}} for the {{% gl node %}}. To determin which you
can use with your particular {{% gl NodeDriver %}}, run:

```
% feditest info --nodedriver FediverseSaasNodeDriver
```

