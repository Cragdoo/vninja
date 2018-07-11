---
author: Christian Mohn
comments: true
date: 2011-06-12 20:34:52+00:00
layout: post
slug: multi-hypervisor-management-and-the-future
title: Multi-Hypervisor Management and the Future
url: /news/multi-hypervisor-management-and-the-future/
wordpress_id: 1158
categories:
- News
- Rant
- Virtualization
tags:
- Hyper-V
- hypervisor
- management
- Tech Field Day
- vCenter
- Xen
---

In a previous post, [vCenter Integration Mantra](http://vninja.net/news/vcenter-integration-mantra/), I made the point that vSphere vAdmins wants the 3rd party modules to integrate into the vCenter client and show their delicious addon-value there, and not in their own management interface. Give the vAdmins the info they need, where they do most, if not all their work. Open up the admin client and let us get all that juicy and fruity information we need. Sounds good, right?

Yes, it does. It sounds really good, but there is this one small curve-ball that can change everything. The 500 pound gorilla in the room that no-one wants to talk about, but we all know is there. As a day-to-day VMware vSphere admin it’s really easy to get our blinders on and not see the forest for all the trees.

During [Embotics](http://www.embotics.com/) presentation at [Tech Field Day #6](http://techfieldday.com/2011/tfd6/)  in Boston, it dawned on me:

**We might just be approaching this entirely the wrong way around.**

This epiphany was caused by one single statement from Embotics: “Our plan is to be hypervisor agnostic, and support other architectures in future versions”. 

Multi-hypervisor support? Provisioning VMs regardless of hypervisor, just create what the business or application owner needs, on the performance and storage tier that suits that particular usage pattern best. Of course, this very much ties into the whole cloud mindset, and as a concept of management it is really interesting.



#### Consider the following scenario:


Your enterprise has a high-performance VMware vSphere environment where mission-critical applications run. All the bells and whistles are available in this environment; HA/DRS, High Performance SAN, 10GigE networking and loads of CPU and RAM. 
For a given set of workloads available in the service catalog, the default would be to create new mission-critical workloads in this environment.  Of course, chargeback mechanisms would also come into play, and price workloads in this environment at a premium level.
For the sake of simplicity we’ll call this tier _“Tier 1”_ 

_“Tier 2”_ would be your test/development and QA environment where you probably won’t need the performance and high availability you get in “Tier 1”, and the chargeback mechanisms would reflect this in the cost model. This environment runs Hyper-V, has cheaper storage and simpler networking.

_“Tier 3”_ is your hosted environment, available outside of your own datacenters. Some workloads belong here too, and of course, chargeback would come into play here as well. To further complicate things, the provider uses Xen for their environment.

If 3rd party applications where to tie into the administration tools for those three separate hypervisors, administrators would have to use three different tools to manage their environment.

What if we turn everything on its head, and look at it from the exact opposite direction. Why should 3rd party vendors have to tie into the hypervisors management tools? They wouldn’t have to if the hypervisor vendors made their admin tools available in a manner that let 3rd party vendors integrate their management tool into theirs instead? 

Solarwinds does something really interesting in their [Virtualization Manager](http://www.solarwinds.com/products/virtualization-manager/) product, the statistics and status reports you get in your Solarwinds Virtualization Manager dashboard are all available as web “widgets” that you can include in other web pages. In other words, you can integrate Solarwinds Virtualization Manager data into your existing dashboards. 

What if you could do the same with output from future VMware vSphere, [Hyper-V](http://www.microsoft.com/windowsserver2008/en/us/hyperv-main.aspx) and [Citrix XenServer](http://www.citrix.com/English/ps2/products/product.asp?contentID=683148) management products? [VKernel](http://www.vkernel.com/) could do the same for their data, and you could easily create your own dashboard that contained information from a wealth of sources. 

Of course, there are a number of issues with doing something like this, like “what happens if you want to move something from Tier 2 to Tier 1, and the VMs run on different hypervisors?”, “How do you enforce security between hypervisors with different management systems?” and so on.

This is not something that can be very easily done today, and it might even be a pipe-dream, but it’s an intriguing thought. I do think we will see more and more multi-hypervisor environments in the years to come, and getting into the market for management of such environments seems like a good business opportunity (Note: IANAA = I Am Not An Analyst).

Of course, this is an overly simplified scenario, but it does show that the need for management tools to be hypervisor agnostic is very much present, and will be even more so in the future.

**We as vAdmins need to apply pressure on our hypervisor vendors to try and make them open up their management tools in such a way that this could be possible someday, the multi-hypervisor world is already here and it’s growing.**

**Note:** 
I wrote this post while on the plane returning from Tech Field Day #6 in Boston. [Greg Ferro](http://twitter.com/etherealmindd) has replied to my original post with a post of his own [vCenter Integration Mantra – vEverything Is Not Wise](http://etherealmind.com/vcenter-integration-mantra-veverything-is-not-wise/) where he pretty much says the same as I do in this post. 

[Tom Howarth](http://twitter.com/tom_howarth ) also [commented on the original post](http://vninja.net/news/vcenter-integration-mantra/#comment-2836). In fact, I even voiced this opinion in the session we had with Embotics, when the Tech Field Day delegates had a roundtable discussion after the presentations.

It all goes to show that you can in fact get blinded by the light. 
