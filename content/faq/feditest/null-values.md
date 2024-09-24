---
title: FediTest emits many "null" values in the generated JSON files. Why is that?
---

That's a side effect of the [`msgspec` JSON library](https://jcristharif.com/msgspec/)
we are using. There does not seem to be an obvious way of turning those off.

On the other hand, the advantage of the many `null` values is that you can immediately
see in the JSON where there are optional settings that you may want to fill in.
