---
title: Community-contributed test results
breadcrumbtitle: Results
---

The FediTest project does not produce official testing results. (See
{{% pageref "/blog/2024-06-13-some-tentative-decisions.md" "this post" %}} on why.)
But we may link to some interesting results produced by the community here.

Have some? {{% pageref "/" "Catch us" %}} in the Fediverse or on Matrix.

## June 16, 2024 (Steve Bate and Johannes Ernst)

Work in progress, based on {{% pageref "/release-notes/0.3.md" "FediTest V0.3" %}}.

* **[WebFinger Server tests of 94 Fediverse SaaS applications](/contrib/results/2024-06-16/)**.

## October 24, 2024 (Johannes Ernst)

Work in progress, based on {{% pageref "/release-notes/0.4.md" "FediTest V0.4" %}}.

* **NEW: [2-Node Fediverse System Tests](/contrib/results/2024-10-24T22.02.04/system2-all.testresult.html)**,
  testing all combinations of 1) Mastodon and 2) WordPress (with the needed plugins) against
  each other. This is work in progress, not all the issues shown may remain real issues once
  we have discussed them with the relevant implementors.

  Want to run the tests yourself? Use the setup described in {{% pageref "/quickstart/evaluate/" %}}
  (60-90 min section). Specify the one {{% gl constellation %}} described there (that gives you
  the first column in these results) or several, as we did for these results.

* **NEW: [3-Node Fediverse System Tests](/contrib/results/2024-10-24T22.02.04/system3-mastodon-mastodon-mastodon.testresult.html)**,
  running three Mastodon instances against each other for some tests that require 3 {{% gls node %}}.

  These are the results from the setup described in {{% pageref "/quickstart/evaluate/" %}}
  ("Feeling adventurous?").

* Update to the **[WebFinger Server tests](/contrib/results/2024-10-24T22.02.04/webfinger-server-all.testresult.html)**
  from June.
  **Note**: there's a regression in FediTest 0.4 where the titles and labels of columns
  do not reflect the tested {{% gl app %}}, so it's a bit harder to figure out
  what is what. Sorry about that. Will be fixed in
  [FediTest 0.5](https://github.com/fediverse-devnet/feditest/milestone/7).