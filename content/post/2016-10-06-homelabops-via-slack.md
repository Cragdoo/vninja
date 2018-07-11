---
author: cmohn
comments: true
date: 2016-10-06 10:52:09+00:00
layout: post
slug: homelabops-via-slack
title: HomeLabOps via Slack
url: /homelab/homelabops-via-slack/
wordpress_id: 4222
categories:
- Homelab
tags:
- Alerting
- fun
- Slack
- Webhook
---

Just like [Lior Kamrat](http://imallvirtual.com/vrops-and-slack-collaborate-your-ops/) I've set up my own private Slack for messaging and alerting from various services running both in my lab and some external facing services.  It's only been running a few days, but so far it works brilliantly and helps me keep track.

<!--more-->

So far, I've set up Slack alerting and/or integrations for the following items:


  * [StatusCake](https://www.statuscake.com) monitoring for vNinja.net and other public facing web services


  * [Todoist integration](https://blog.todoist.com/2016/05/05/todoist-slack-integration/)


  * [Pocket](https://getpocket.com/): New items added to Pocket gets announced via Zapier.


**Incoming Slack webhooks**


* Windows application that sends status messages

* [phpipam](http://phpipam.net): If a new device is detected in my [local network, send a notification](http://vninja.net/virtualization/adding-slack-notifications-to-phpipam/)

* Jumphost: Just like [Luca](http://www.virtualtothecore.com/en/tunnel-all-your-remote-connections-through-ssh-with-a-linux-jumpbox/) I have a public facing Linux JumpHost that I tunnel traffic through. I've set up an alerting mechanism that sends a [Slack notification if someone logs into the JumpHost](http://vninja.net/homelab/logging-ssh-logins-to-slack/).

* [If my public IP changes](http://vninja.net/homelab/cloudflare-dynamic-dns-update-script-cf-ddns-sh/), it gets updated and Slack gets notified.

* Wordpress events, like new posts or comments are also announced via the [WP-Slack](http://gedex.web.id/wp-slack/) plugin.



I'm sure I'll add other things to this as time passes. I plan on publishing something on how I've hacked some of this together,  I just need to clean up the code a bit and make it ready for publication first.
