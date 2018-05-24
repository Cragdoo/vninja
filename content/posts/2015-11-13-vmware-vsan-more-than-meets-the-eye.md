---
author: cmohn
comments: true
date: 2015-11-13 08:54:01+00:00
layout: post
slug: vmware-vsan-more-than-meets-the-eye
title: VMware VSAN - More than meets the eye.
url: /virtualization/vmware-vsan-more-than-meets-the-eye/
wordpress_id: 3892
categories:
- Virtualization
tags:
- Future
- Microvisor
- VSAN
- vSphere
---

Way back in 2014 I wrote a piece called [VSAN – The Unspoken Future](http://vninja.net/virtualization/vsan-unspoken-future/), and I think it's about time it got a revision. Of course, lots of things have happened to VSAN since then and even more is on the way, but I think there is more to this than adding features like [erasure coding, deduplication](http://www.yellow-bricks.com/2015/09/01/virtual-san-beta-coming-up-with-dedupe-and-erasure-coding/) and compression. All of these are important features, and frankly they need to be in a product that aims a lot higher than you might think.



At the moment, VSAN does storage internally in a vSphere Cluster. If you want to use that storage in other ways, you either have to share it from a VM over the network or use [NexentaConnect for VMware Virtual SAN](https://nexenta.com/products/nexentaconnect/nexentaconnect-vsan).

Yesterday, [VMUG.it ](https://twitter.com/vmugit/status/664759077059371009)shared the following photo from [Duncan Eppings](http://twiitter.com/DuncanYB) "**Goodbye SAN Huggers, Hello Virtual SAN**" session from their VMUG UserCon:

[![Generic Object Storage Platform](http://vninja.net/wordpress/wp-content/uploads/2015/11/CTmy15VWwAAY8Y9.jpg-large-1024x576.jpeg)](http://vninja.net/wordpress/wp-content/uploads/2015/11/CTmy15VWwAAY8Y9.jpg-large.jpeg)

Look closely at that one for a minute. What is Duncan and VMware telling us here, if you squint your eyes and try to read between the lines? For me, this slide was a bit of a lightbulb moment:** VMware wants to turn VSAN into a generic storage provider in the data center. **You need some storage of some sort? VSAN will provide it, even if your applications are not located on the same cluster.  Object based? Sure. Block? Sure. REST? Sure, that's what the cool kids do. VMFS? Only if you need to run a VM.

Couple this with  the [vSphere Integrated Containers and Photon Platform announcements](http://www.vmware.com/radius/vmworld-2015-the-end-of-the-beginning-lets-go/), VMware is already talking about the microvisor. So, remove the vSphere layer in the slide above, and replace it with a variant of the [VSAN ROBO witness appliance](http://cormachogan.com/2015/09/11/a-closer-look-at-the-vsan-witness-appliance/) of some sort, which runs enough to provide policy based storage services. Once you have those two bits talking to each other, you don't need the traditional vSphere layer to provide hardware virtualization at all for those cloud native apps. Add NSX to the mix, and network policies that follow the application, and you have a portable application infrastructure that can run pretty much anywhere you prefer.  At VMworld 2015, VMware showed NSX for Multi-Hypervisor running on AWS, extending the network from on-premises to Amazon. Why not do the same with storage?  Want cloud based storage? Sure, add the little VSAN layer in front of your providers storage offering, and boom, instant policy  management and portability.

And of course, VMware will be there to provide you with the management and monitoring layer for all of this - Even if you don't run vSphere.

VMware is getting ready for the post-virtualization, multi-platform, world, no question about it.  Are You Ready for Any?


