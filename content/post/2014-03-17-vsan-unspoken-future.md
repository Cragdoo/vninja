---
author: Christian Mohn
comments: true
date: 2014-03-17 22:41:34+00:00
layout: post
slug: vsan-unspoken-future
title: VSAN - The Unspoken Future
url: /virtualization/vsan-unspoken-future/
wordpress_id: 3117
categories:
- Virtualization
tags:
- Data Center
- featured
- Form Factor
- Node
- SDDC
- Server
- Virtualization
- VSAN
---

This rather _tongue-in-cheek_ title, is a play on [Maish ](https://twitter.com/maishsk/)recent [VSAN - The Unspoken Truth](http://technodrone.blogspot.com/2014/03/vsan-unspoken-truth.html) post where he highlights what he thinks is one of the hidden "problems" with the current version of VSAN is it's inherent non-blade compatibility and current lack of "rack based VSAN ready nodes".

Of course, this is a reality; If you base your current infrastructure on blade servers, [VSAN](http://www.vmware.com/products/virtual-san) probably isn't a good match as it stands today. Chances are that if you are currently running a blade-based datacenter, you have traditional external storage on the back end of that, and that you for quite some time will be running a form factor that VSAN simply isn't designed for. I don't disagree with Maish in that conclusion, not a bit.
<!--more-->

But what about the next server refresh? One of the things that VSAN is a facilitator for, along with enhancements in the storage industry, is the ability to move to other form factors. Currently Supermicro offers their rather nice looking[ FatTwin™ Server Solution](http://www.supermicro.nl/products/nfo/FatTwin.cfm). If we look at what the [SYS-F617R2-R72+](http://www.supermicro.nl/products/system/4U/F617/SYS-F617R2-R72_.cfm) box offers, in the total rack space of 4U (less than most blade chassis), it is clear that the form factor choices will not just be tower or blade, the will also include other new form factors that are currently not in the forefront of peoples minds when designing their data center.

Looking at the Supermicro box again, in a 4U rack footprint, it offers these maximums pr node:



  * 2 x Intel® Xeon® processor E5-2600


  * Up to 1TB DDR3 ECC LRDIMM


  * 6x Hot-swap 2.5" SAS2/SATA HDD trays



**So, in 4U, you can get 16 CPU, 8TB RAM and 48 SAS2/SATA bays.  Stick a couple of those in your rack, with a few 10GbE ports, and then try to do something similar with a blade infrastructure!**

Now, of course, VSAN isn't for everyone, nor is it designed to be. In a way VSAN offers a peak to the future of datacenter design, in the same way that it shows us that Software Designed Data Center (SDDC) is not just about the software, it's about how we think, manage AND design our back-end infrastructure. It's not just storage vendors that need to take heed and look at what they are offering, the same also goes for "traditional" server/node vendors.

That's right, a server is becoming a node and which vendors sticker is in the front, might not matter that much in the future.



<blockquote>The future is already here – it's just not evenly distributed.
<br/>— William Gibson</blockquote>



[Header photo credit: [www.LendingMemo.com](www.LendingMemo.com)]
