---
author: Christian Mohn
comments: true
date: 2016-08-13 09:45:13+00:00
layout: post
slug: cloudflare-dynamic-dns-update-script-cf-ddns-sh
title: CloudFlare Dynamic DNS Update Script (cf-ddns.sh)
url: /homelab/cloudflare-dynamic-dns-update-script-cf-ddns-sh/
wordpress_id: 4173
categories:
- Homelab
tags:
- Bash
- Cloudflare
- Dynamic DNS
- Homelab
- Scripting
---

As a part of my[ Homelab](http://vninja.net/homelab/) project, I've created a proper bash script to provide dynamic DNS updates for external resources, via CloudFlare. More details on the reasoning behind it can be found in [Using CloudFlare for Dynamic DNS](http://vninja.net/homelab/using-cloudflare-for-dynamic-dns/), but since that was posted I've fleshed the script out quite a bit more.
<!--more-->

The new and updated script can be found on GitHub: [cf-ddns.sh](https://github.com/h0bbel/homelab/blob/master/code/cf-ddns.sh). It now writes events to syslog when it runs, so you can use [VMware Log Insight](https://www.vmware.com/products/vrealize-log-insight.html) (or another log solution) to capture the events, and use it to track public IP changes if you want. I've also added a _-f_ parameter to it, so you can force an IP update even if the values have not changed since the last run.

It's pretty self explanatory, just replace your own values from CloudFlare for the variables, and if you want to update more than one record, just copy the update block and edit it for the extra entries.

Hopefully someone will find this useful.
