---
author: cmohn
comments: true
date: 2014-09-22 21:48:40+00:00
layout: post
slug: hyperconverged-be-all-end-all-no
title: Is Hyperconverged the be-all, end-all? No.
url: /rant/hyperconverged-be-all-end-all-no/
wordpress_id: 3404
categories:
- Rant
tags:
- Architecture
- Future
- Hyperconverged
- Intel
---

First off, this is not meant to be a post negating the value of current hyperconverged solutions available in the market. I think hyperconverged has it's place, and for many use cases it makes perfect sense to go down that route. But the idea that everyone should go hyperconverged and all data should be placed on local drives, even if made redundant inside the chassis and even between chassis, is to be blunt, a bit silly.

Parts of this post is inspired by a recent discussion on Twitter:

https://twitter.com/h0bbel/status/513379995155447808

I don't believe that replacing your existing storage array with a hyperconverged solution, regardless of vendor, by moving your data off the array and on to the local disks in the cluster makes that much sense. Sure, keep your hot and fresh data set as close to the compute layer as possible, but for long time archiving purposes? For rarely accessed, but required data? Why would you do that? Of course, going hyperconverged would mean that you can free up some of that costly array space and leave long term retention data on the array, but does the hyperconverged solution of choice let you do that? Does it even have FC HBA's? If not, is it cost effective to invest in it, while you at the same time need to keep your "traditional" infrastructure in place to keep all that data available?

To quote [Scott D. Lowe](http://www.hyperconverged.org/dense-server-storage-hyperconverged/):



<blockquote>Any solution that uses standalone storage is not hyperconverged. With a hyperconverged solution, every time a new node is added, there is additional compute, storage, and network capability added to the cluster. Simply adding a shelf of disks to an existing storage system does not provide linear scalability and can eventually lead to resource constraints if not managed carefully.</blockquote>



Doesn't that really show one of biggest the problems with a hyperconverged infrastructure? If you need to scale CPU, Memory AND Storage at the same time, it makes perfect sense. But what if you need to scale one of the items? Individually? Why should you have to buy more CPU, and licenses, if all you wanted was to add some more storage space?

Of course, this brings the discussion right back to where it started, if you want to scale the various infrastructure components individually, then hyperconverged isn't the right solution. But if hyperconverged isn't the solution, and traditional "DIY" infrastructures have to many components to manage individually, then what? Sure, the Software Defined Data Center looks promising, but at the end of the day, we still need hardware to run the software on. The hardware may very well be generic, but it's still required.

Interestingly enough, a post by Scott Lowe (no, not the same one as quoted above), got me thinking about what the future might hold in this regard: [Thinking About Intel Rack-Scale Architecture](http://blog.scottlowe.org/2014/09/22/thinking-about-intel-rack-scale-architecture/). To get to the point where we can manage a datacenter like a hyperconverged cluster, and still be able to scale vertically as needed, we need a completely new approach to the whole core architecture of our systems. Bundling CPU, Memory, Storage and Networking in a single manageable unit doesn't cut it in the long run. Now that the workloads are (mostly) virtualized, it's time to take a real hard look at how the compute nodes are constructed.

Decoupling _CPU_, _Memory_, _Storage Volume, Storage Performance_ and _Network_ into entirely modular units that can be plugged in and scaled individually makes a whole lot more sense. By the looks of it _Intel Rack-Scale Architecture_ might just be that, I guess we'll see down the road if it´s actually doable.

The software side of things are moving fast, and honestly, I'm kind of glad that hardware isn't moving at the same pace. At least that gives us breathing room enough to actually think about what we're doing, or at the very least pretend that we do.
