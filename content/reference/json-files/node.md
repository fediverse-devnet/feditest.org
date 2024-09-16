---
title: Node definition syntax
---

A {{% gl Node %}} definition captures everything FediTest needs to know to interact
with, and --potentially -- provision and deprovision an {{% gl app %}}.

The syntax is as follows:

```
{
    "nodedriver" : "<name>",
    "parameters" : {
       "<key>" : "<value>",
       ...
    },
    "accounts" : [
        {
            "<key>" : "<value>",
            ...
        },
        ...
    ],
    "non_existing_accounts" : [
        {
            "<key>" : "<value>",
            ...
        },
        ...
    ]
}
```

Field `nodedriver`:
: The name of the {{% gl NodeDriver %}}. This field is required.

Field `parameters`:
: Additional information about how the {{% gl App %}} at the {{% gl Node %}} is, or shall be,
  configured. For example, almost all {{% gls NodeDriver %}} support the `hostname`
  parameter. This field is optional, unless a {{% gl NodeDriver %}} requires certain
  entries.

Field `accounts`:
: Lists the accounts that already exist on this {{% gl Node %}} and that FediTest may
  use without having to create them first. This field is optional. This is useful if
  you want to use an already-running {{% gl SaaS %}} {{% gl Node %}} for testing on which
  you manually created certain accounts that can be used for testing.

Field `non_existing_accounts`:
: Lists some "accounts" that do not exist on this {{% gl Node %}} but could. This is useful
  for tests that require an account not to exist.

To determine...

* ... the available {{% gls NodeDriver %}}, run `feditest list-nodedrivers`.

* ... what a given {{% gl NodeDriver %}} does, and what `parameter`s and `account`
and `non_existing_account` parameters it understands, run `feditest info --nodedriver <name>`.
