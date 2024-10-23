---
title: "What report formats are supported?"
---

FediTest reports on passing and failing tests, as well as other errors, in several ways:

* **On the terminal:** problems and errors are reported on `stderr`. By using the `--verbose`
  flag, progress is also reported to `stdout`.

* **JSON:** running `feditest run --json <file>` will provide a full transcript of the test
  run in JSON format. Use this to post-process or produce custom reports for your own
  purposes.

* **TAP:** currently there is also the `feditest run --tap <file>` option, but we may remove
  it in the future as the JSON output is much more detailed and can be processed more
  easily than TAP format.

* **HTML**: running `feditest run --html <file>` will provide a report in HTML. Depending
  on the {{% gl testrun %}}, the report may be one ore more HTML files.
