---
author: Christian Mohn
comments: true
date: 2011-11-22 22:59:30+00:00
layout: post
slug: testing-vmware-vsphere-5-swap-to-host-cache
title: Testing VMware vSphere 5 Swap to Host Cache
url: /vmware-2/testing-vmware-vsphere-5-swap-to-host-cache/
wordpress_id: 1630
categories:
- VMware
tags:
- Experiment
- Host Cache
- SSD
- VMware
- vSphere 5
---

A little while ago I fitted a small 64GB SSD disk to my HP MicroServer just to have a quick look at the new vSphere 5 feature _Swap to Host Cache_, where vSphere 5 reclaims memory by storing the swapped out pages in the host cache on a solid-state drive. Naturally, this is a lot faster than swapping to non-SSD storage, but you will still see a performance hit when this happens. For more details on Swap to Host Cache, have a look at [Swap to host cache aka swap to SSD?](http://www.yellow-bricks.com/2011/08/18/swap-to-host-cache-aka-swap-to-ssd/) by [Duncan Epping](http://twitter.com/#!/DuncanYB).

Now, in my miniscule home lab setting it's somewhat hard to get some real tangible performance metrics, so my little experiment is non-scientific and only meant to illustrate how swap to host cache gone wild would look in a real world environment.

After installing the SSD drive, and configuring Swap to Host Cache, I created two VMs ingeniously called _hostcacheA_ and _hostcacheB_. Both were configured with 14GB of memory, which should nicely overload my host that has a whopping 8GB of memory in total.

[![](/img/hostcache-300x48.png)](/img/hostcache.png)

[![](/img/1-300x196.png)](/img/1.png)

Now, with memory features like _ballooning_, _transparent page sharing_, and _memory compression_ I needed to make sure that the actual memory was used, and in addition it had to contain different datasets to make sure that the host cache actually kicked in.

To make sure of this, I downloaded the latest ISO version of [Memtest86+](http://www.memtest.org/) and connected it to the empty VMs. 

[![](/img/2-300x267.png)](/img/2.png)

When starting the VMs, they immediately started testing their available memory and sure enough, they started eating into the host cache.

[![](/img/3-300x166.png)](/img/3.png)

As you can see from the screenshot below, the longer the memtest ran the more host cache was utilized. 
_Bonus points for figuring out when the test VMs were shut down..._

[![](/img/Problem2-300x137.png)](/img/Problem2.png)

So there it is, performance graphs showing that the host cache is indeed kicking in and getting a run for it's money. Since this was a non-scientific experiment, I don't have any real performance counters or metrics to base any sort of conclusion on. All I was after was to see if it came alive, and clearly it did.
