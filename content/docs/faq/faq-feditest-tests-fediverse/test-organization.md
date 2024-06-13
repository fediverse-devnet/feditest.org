---
title: Why are there separate test sets in directories such as webfinger, activitypub and fediverse?
---

* Protocols such as WebFinger can be, and are used, in many more situations than just for the Fediverse. So we have a directory `webfinger` that (only) contains tests derived from the IETF standard that defines WebFinger. Similar directories exist for other standards, such as ActivityPub.

* There are other documents that define the particularities how particular standards are used in today's Fediverse, e.g. a Social Web Community Group report on WebFinger. Tests derived from those are defined in corresponding directories, e.g. `swicg-activitypub-webfinger`.

* The directory `fediverse` contains tests that test what the user of today's Fediverse expects, such as that when they follow another account, they will receive their posts. Perhaps surprisingly, the behaviors in this directory are actually not standardized anywhere.
