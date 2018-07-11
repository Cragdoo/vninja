---
author: cmohn
comments: true
date: 2015-07-13 22:17:12+00:00
layout: post
slug: time-add-insanity
title: It's Time to End the Add on Insanity
url: /rant/time-add-insanity/
wordpress_id: 3699
categories:
- Rant
tags:
- Adobe
- Flash
- Rant
- Security
---

<blockquote>For the third time in a week, researchers have discovered a zero-day vulnerability in Adobe’s Flash Player browser plugin. Like the previous two discoveries, this one came to light only after hackers dumped online huge troves of documents stolen from Hacking Team — an Italian security firm that sells software exploits to governments around the world.</blockquote>

<!--more-->


This quote is from [Brian Krebs](http://Third Hacking Team Flash Zero-Day Found), who very rightfully goes on to advise that everyone _"please consider removing or at least hobbling this program."_ Now, that is fine for the most part. I mean, who really needs Adobe Flash these days? Don't most services we use have other methods of handing us the content we <del>need</del> want? The [Apple iPhone doesn't have Adobe Flash](https://www.apple.com/hotnews/thoughts-on-flash/), so why do we need it on our laptops?

The fact is, that most end users probably don't need to have Adobe Flash installed any more, but a lot of us sysadmins do. Why? Well, in my world one major culprit is the VMware vSphere Web Client. The Web Client has gotten it's fair share of ill-repute over the last few years, but the latest edition in vSphere 6 is pretty responsive and quite pleasant to use. That's until you contemplate that it still needs Adobe Flash installed on the client. **The same goes for any other admin interface that requires Adobe Flash, or even Java for that matter.**

_Any administrative interface that requires a browser add on to work, should be bagged, kidnapped and flung in the back of a van and driven off somewhere never to be seen again._ Sure, I understand that it's no easy task to rework all of these interfaces, and it takes real effort by skilled people. But please, please make it happen as soon as possible, and retrofit it it into your existing systems - don't keep those stuck on older releases hanging, and only provide a solution for the latest and greatest version.

While we as admins and consultants are used to having to patch our systems, and keep current, please help us limiting our own attack surface by removing requirements for add ons and "special juice" just to be able to administer the solutions we depend upon to keep our businesses running. That can't be too much to ask, can it?
