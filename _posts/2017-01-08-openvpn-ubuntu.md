---
title:  "OpenVPN, Ubuntu 16.04, and WiFi connection problems"
date:   2017-01-08
tags: openvpn ubuntu wifi networking troubleshooting
---

I spent quite some time over the last few weeks to debug a strange issue that happened when connecting to a (newly configured) OpenVPN server with Ubuntu 16.04. Every few minutes the VPN connection simply dropped, with this log message:

{%  highlight text %}
Inactivity timeout (--ping-restart), restarting
{% endhighlight %}

The problem occurred sporadically, and nothing (firmware update on router, 'polling' a server inside the private network to generate traffic, RTFM, etc.) helped.

Yesterday I finally found a workaround in [this AskUbuntu answer](http://askubuntu.com/a/689492/284477) for a similar connection problem: after editing the WiFi connection and setting the IPv6 settings to `Ignore`, everything seems to work fine (for now).
Since there are [other workarounds](https://silverdrs.wordpress.com/2015/12/01/openvpn-inactivity-timeout-ping-restart-restarting/) for problems with the same symptoms, I suggest trying this first, since it is a simple and quick workaround.

OpenVPN support on Ubuntu can be rather challenging[^1] to set up, depending on the server configuration. 

Here is what I learned about that in the last weeks:

- Depending on your configuration file, [import into the Ubuntu network manager may not work](http://askubuntu.com/q/760345/284477), so you should check if there is a known [bug](https://bugs.launchpad.net/ubuntu/+source/network-manager-openvpn/+bugs) that affects you, e.g. missing support for certain OpenVPN options.

- Even when you got the import working, the network manager may simply not support all settings you need. In this case, you will have to run OpenVPN manually: `sudo openvpn path/to/my/config.ovpn` (this is also useful for debugging).

- [Some issues](https://silverdrs.wordpress.com/2015/12/01/openvpn-inactivity-timeout-ping-restart-restarting/) are caused by multiple machines being connected to the same OpenVPN server with the same certificate. Only use a single session to rule out any problems in that regard.

- WiFi can cause a lot of trouble for OpenVPN and you may not notice very brief connection interruptions yourself, so improve your router configuration if you suspect this to be a problem (e.g. switch off the 5GHz band, only use 802.11b+g, etc.).

- DNS setup also needs an extra step that may be missing from your OpenVPN config file. It [should contain](http://askubuntu.com/a/845542/284477):

{% highlight text %}
script-security 2
up /etc/openvpn/update-resolv-conf
down /etc/openvpn/update-resolv-conf
{% endhighlight %}

- For more troubleshooting, change the verbosity level in your config file (e.g. to `verb 4`) and try again.

Good luck!

[^1]: When I configured my clients for the previous VPN setup, it was the other way around: a nightmare on Windows, but a piece of cake on Ubuntu. I guess it really *does* depend on the specific OpenVPN setup.