---
title: "Q: I like to run my application locally on a non-standard port during development.
       How do I test it with feditest?"
breadcrumbtitle: Non-standard port
---

Running tests directly against non-standard ports is out of scope for FediTest.

However, you can easily work around this. Simply run a web server (like nginx, or
Apache -- pre-installed on macOS) locally on a standard port and configure it as a
reverse proxy to forward requests from the standard port to your application at the
non-standard port. How do to that is widely documented online. Some Fediverse
applications -- like Mastodon -- assume that as a default setup anyway.

It may be advantageous to run the reverse proxy just for one name-based virtual host.
Then you can run other applications as well on the same system with the same web server
at different virtual hostnames, but all using standard ports.

It also allows you to easily add TLS certificates to your setup as you can simply
add them to your webserver's configuration.

P.S. Sounds anything but simple? It actually is quite straightforward once you have
done it once. In doubt, find us [in chat](/about/#chat).