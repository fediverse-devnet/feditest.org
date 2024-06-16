---
title: Run the WebFinger server tests with your application running anywhere (including localhost)
breadcrumbtitle: Run the WebFinger server tests with your application
weight: 10
---

Your application can run anywhere, as long as FediTest running on your development machine
can perform HTTP GET requests on it. For example, your application can be installed on your
development machine if you simply have an alias `example.com` to `127.0.0.1` in your
`/etc/hostname`; no need to put it on a real server. You can pick any DNS name you like as
long as you also use it below.

{{% include-md file="includes/install/local.md" %}}

* Run the following command:

  ```
  $ feditest run --testplan examples/testplans/webfinger-server-imp-vs-saas-any.json --html myapp-webfinger-results.html
  ```

  It will ask you a few questions (you are running a generic {{% gl TestPlan %}} after all).
  Make sure to enter account identifiers with an `acct:` scheme, such as `acct:myaccount@example.com`.

* Open the generated HTML file `quickstart-results.html`. You find the tests whose results
  you just opened in directory `tests/webfinger/server`.

### To automate this further

If you run this {{% gl TestPlan %}} multiple times, you certainly don't want to re-enter
the same information every time. So you modify the {{% gl TestPlan %}} you referenced in
the following way:

* Copy the JSON file containing the {{% gl TestPlan %}} you just ran:

  ```
  % cp examples/testplans/webfinger-server-imp-vs-saas-any.json myapp-webfinger-testplan.json
  ```

* In the copy, find the section that defines the test {{% gl Constellation %}} (look for
  keyword `constellation`) and in it, the section that defines the non-Imp {{% gl Node %}}
  called `server`. It looks like this:

  ```
          "server": {
              "nodedriver": "saas.SaasFediverseNodeDriver",
              "parameters": null
           }
   ```

* Replace it with a `server` section that looks like the following. You just need to add a
  `parameters` section:

  ```
          "server": {
              "nodedriver": "saas.SaasFediverseNodeDriver",
              "parameters": {
                  "app": "MyApp",
                  "hostname": "example.com",
                  "existing-account-uri": "acct:testuser@example.com",
                  "nonexisting-account-uri": "acct:does-not-exist@example.com"
              }
          }
  ```

  Use the values that apply to how you have installed your application.

* Then, re-run the test with this updated {{% gl TestPlan %}}:

  ```
  $ feditest run --testplan myapp-webfinger-testplan.json --html myapp-webfinger-results.html
  ```

  No more repetitive interactive prompts!

### Drilling down on one failing test

Let's say one of the WebFinger tests is failing, and you want to debug what is going on.
In this case, you want to run that one test only for a while, and not run all the other
tests that are passing. How could you do that?

The same way! You modify your {{% gl TestPlan %}}:

* Make another copy of the {{% gl TestPlan %}} JSON:

  ```
  % cp myapp-webfinger-testplan.json myapp-webfinger-focus.json
  ```

* In the copy, find the section that has the tests: look for keyword `tests`. Delete all
  the entries in this array that aren't the test you want to focus on. Then, run your
  updated {{% gl TestPlan %}}:

  ```
  $ feditest run --testplan myapp-webfinger-focus.json --html myapp-webfinger-focus-results.html
  ```

This approach also lets you define your very own {{% gl TestPlan %}} for, say, continuous
integration purposes.