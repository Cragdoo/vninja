---
author: Christian Mohn
comments: true
date: 2016-10-10 13:18:30+00:00
layout: post
slug: patch-those-dell-servers-easily
title: Patch those Dell Servers easily!
url: /hardware/patch-those-dell-servers-easily/
wordpress_id: 4244
categories:
- Hardware
tags:
- Dell
- Patching
- Server
- Update
---

![screenshot-2016-10-10-15-13-59](/img/Screenshot-2016-10-10-15.13.59-300x181.png)Did you know that Dell has a bootable linux ISO with firmware upgrades for their servers available? Neither did I, but luckily I found it today when needing it at a customer site.

<!--more-->

Check out [Updating Dell PowerEdge servers via bootable media / ISO](http://www.dell.com/Support/Article/us/en/04/SLN296511) where you can download bootable ISOs for specific server models, and get all your firmware upgraded in one fell swoop.

Be warned though, it takes quite some time to run though it all (125 update packages in this instance), but it sure beats installing manually or creating your own bootable ISO with the Dell Repository Manager or even using Dell Server Update Utility. Especially when it's been a while since the server has been patched, the ones I used this on had not been touched since 2012...

Fire and forget via iDRAC, I can like that. Just make sure you update the iDRAC before you boot the ISO, or you might just get disconnected mid-way, and have to start over.
