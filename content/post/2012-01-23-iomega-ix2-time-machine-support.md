---
author: cmohn
comments: true
date: 2012-01-23 19:57:49+00:00
layout: post
slug: iomega-ix2-time-machine-support
title: Iomega IX2 and Time Machine Support
url: /osx/iomega-ix2-time-machine-support/
wordpress_id: 1735
categories:
- OS X
tags:
- Firmware
- Hack
- Iomega
- IX2
- Mac
- OS X
---

As Mr. [Simon Seagrave](http://twitter.com/Kiwi_Si) has pointed out, there is a fix available to enable [OSX Lion Time Machine support for Iomega IX2 and IX4 NAS storage devices](http://www.techhead.co.uk/iomega-ix2-ix4-os-x-lion-time-machine-fix).

I decided to take this a little step further, and try to upgrade my old (and discontinued) Iomega IX2-200 to the new [IX2-200 Cloud Edition](http://go.iomega.com/en/products/network-storage-desktop/storcenter-network-storage-solution/network-hard-drive-ix2-200-cloud/?partner=4745) firmware.

Initially this was a big failure, as I seemingly managed to brick my device. It was only responding to pings (so the TCP/IP stack was loaded and working), but I could not bring up the web based management tool nor connect via telnet or SSH.

Thankfully [Will van Antwerpen](https://twitter.com/wilva) had investigated the firmware upgrade to cloud edition a bit more than I had, and pointed me to the [General NAS-Central Forums](http://forum.nas-central.org/viewforum.php?f=243) where I found a link to a great HowTo explaining the entire process: [Upgrading Iomega ix2-200 to Cloud Edition](http://www.technopat.net/forum/ssd-flash-ve-sabit-diskler/2790-upgrading-iomega-ix2-200-cloud-edition.html).

As that article also mentions, I had to do the process twice to get it to kick in and un-brick my IX2-200 and get it running with the new Cloud Edition firmware.

[![](http://vninja.net/wordpress/wp-content/uploads/2012/01/ix2-300x206.png)](http://vninja.net/wordpress/wp-content/uploads/2012/01/ix2.png)

After configuring the IX2 with security and setting up Time Machine on the Macbook Air, Time Machine seems to be running without problems.

[![](http://vninja.net/wordpress/wp-content/uploads/2012/01/timemachine-300x214.png)](http://vninja.net/wordpress/wp-content/uploads/2012/01/timemachine.png)

<MrBurns>Excellent</MrBurns>
