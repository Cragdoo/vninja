---
author: Christian Mohn
comments: true
date: 2011-06-10 15:43:18+00:00
layout: post
slug: vcenter-integration-mantra
title: vCenter Integration Mantra
url: /news/vcenter-integration-mantra/
wordpress_id: 1151
categories:
- News
- Rant
- Virtualization
tags:
- Ops
- realworld
- Tech Field Day
- vCenter
- VMware
- vSphere
---

During [Tech Field Day #6 in Boston](http://techfieldday.com/2011/tfd6/) one particular general feature request has become increasingly prominent; 

**Can we have it inside the vCenter client?**
 
In short, what we want is for all those great third party vendors like [VKernel](http://www.vkernel.com/), [Solarwinds](http://www.solarwinds.com/) and others to be able to put their feature addons directly into the vCenter client. Currently most third party apps "integrate" by offering a new tab where you can access it, but I would love to see that being expanded even further.

As a concrete example, I would love to see for instance VKernel identify performance problems for a VM and then tell me, inside the summary tab for that VM that there is a problem. Show it to me where I do my work, which is in vCenter, and mostly on the summary screen. Dashboards are great, but we're all suffering from dashboarditis, and the more disparate dashboards and tabs we get to have relations with, the less we're actually able to use them properly. 

Also, raise a vCenter alert to leverage my existing alerting scheme to get my attention. If a 3rd party plugin sees that something is wrong, tell me through the existing infrastructure I already have in place. No need to reinvent the wheel each and every time, from each and every vendor.

Couple that with my back-end user authentication (eg. Active Directory) for sign-on into your solution, and we're getting much closer to the "single pane of glass" nirvana scenario we're all craving. 

From what I gather the vCenter client (I am in no way, shape or form a developer), doesn't allow this level of integration on the VM summary screen right now but VMware needs to make it possible in the future. 

**We admins want this, and we need it, and frankly, I think we deserve it (and so does the 3rd party vendors contributing to this ecosystem).**



#### Update:


**[Read Multi-Hypervisor Management and the Future](http://vninja.net/news/multi-hypervisor-management-and-the-future/) for an updated and more forward thinking post on the same subject matter.**
