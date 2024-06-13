---
title: Tests can be defined as functions and as classes with "steps". What's that for?
---

Consider a Fediverse test such as: "if I reply to the post of my friend, they will see my
reply on their system right below their original post."

Let's call myself user `@me@me.example`
on Node `me.example` and my friend `@you@you.example` on Node `you.example`.

To test this, many steps are necessary:

1. Fediverse Nodes `me.example` and `you.example` need to be set up and put into a pristine state.
  This may involve manual software installation or a database wipe.

2. Accounts `@me@me.example` and `@you@you.example` need to be created. This may involve
  clicking through a bunch of web pages and waiting for e-mail to arrive.

3. Account `@you@you.example` needs to create a post.

4. Account `@me@me.example` needs to access that post on `me.example`.

5. Account `@me@me.example` needs to reply to that post.

6. Only now do we get to the actual test: has the reply has arrived on `you.example`?

Let's say the test fails: with a typical test framework, the test raises some kind of
`AssertionError`, the test framework stops the test run, and, as it is supposed to, cleans
up after itself by deleting all the data created during the test, perhaps even taking
down the Nodes.

This is not optimal for the developer who wants to find out what went wrong. It would
be better if, for example, the test run could be stopped right after the failure,
and the developer could repeat just the step before the failure. Here: the developer
would like to repeat step 5, perhaps more than once, maybe with extra logging or
attaching a debugger to a Node, to figure out what's going wrong.

That's why in FediTest, we have:

* a flag for interactive mode: ``feditest run --interactive``
* not just self-contained test functions, but test classes which allow the developer to
  break a test into as many steps as they like. Steps then can be repeated independently
  of each other in interactive mode, as in the above scenario.

So to support this scenario, the developer could define a separate method on their
test class for each step above, annotating them not with `@test` but with `@step`.
In interactive mode, if step 6 fails, they can repeat step 6 (but not the steps before)
or go back to a previous step (say step 5) and repeat that and so forth.

This would not be possible if all the steps were part of the same test function.

By putting the steps into a class, the steps can share state.
In the above example, step 3 might produce a URI to the generated post, which needs
to be given to step 4, so it can access the generated post. As the steps are
separate methods (so they can raise exceptions independently of each other), state
is shared via instance variables in the test class.
