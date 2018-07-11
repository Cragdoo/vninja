---
author: Christian Mohn
comments: true
date: 2010-09-22 13:51:13+00:00
layout: post
slug: vmware-vsphere-hypervisor-licensing-and-cost
title: VMware vSphere Hypervisor Licensing and Cost
url: /virtualization/vmware-vsphere-hypervisor-licensing-and-cost/
wordpress_id: 291
categories:
- Virtualization
tags:
- Ops
- vCenter
- Virtualization
- VMware
---

![](/images/logos/vmware-logo.gif)We all know, and **love**, the fact that vSphere Hypervisor is free of charge. The free version doesn't come with all the bells and whistles of the fully licensed product, but it's still very usable in many scenarios. 

Recently I've been investigating the possibilities of running vSphere Hypervisor on a number of floating branch offices, also known as vessels.

I'm not going into details about the proposed setup, and how we intend to roll it and so on, but one of the things I really wanted to get out of this was to have all my off-site vSphere Hypervisor installs appear in my vCenter Client. I don't need HA, DRS, DPM or any of the other licensed features and I'd be happy with running the free edition if only it was able to connect to an existing vCenter installation.

Sadly, and understandably, this isn't possible in the free edition so I looked into what it would cost to license the installs to make this a possibility. After investigating a bit, it seems that I would need to buy **VMware vSphere Standard** licenses for all the vessels to be able to get what I want.

For 20 vessels, with the standard pricing available on vmware.com, inclusive 1 year support, we come up with this (all prices in USD)
<table cellpadding="0" cellspacing="0" style="height: 48px;" border="0" width="317" >
<tbody >
<tr >

<td width="64" valign="top" >**Hosts**
</td>

<td width="170" valign="top" >**vSphere Standard
License Cost incl. Support**
</td>

<td width="76" valign="top" >**Total Cost**
</td>
</tr>
<tr >

<td width="64" valign="top" >20
</td>

<td width="170" valign="top" >1318
</td>

<td width="76" valign="top" >26360
</td>
</tr>
</tbody>
</table>
In effect, this means that I would need to pay **$26360.- USD **to be able to get my vSphere Hypervisor hosts connected to my existing vCenter. That simply isn't feasible in the current situation. 

Remember, I do not need _any_ of the other features that vSphere Standard provides me with, like Thin Provisioning, High Availability, vMotion, vStorage APIs for Data Protection and Update Manager.  Update Manager could potentially be useful, but that's about it.

So, dear VMware; Have you considered this scenario at all? I'm sure I'm not the only customer looking to deploy vSphere Hypervisor in remote locations, where they will run a single VM and their poor admin wants to be able to have them all managed in a single console?

I would really like to see a "vCenter Connector" license for vSphere Hypervisor, that only provides a way to connect to an existing licensed vCenter instance. 

Is this to much to ask for? I understand that VMware want to get paid for their enterprise products, and I'm normally happy to do so, but in this case the return simply does not warrant the cost.


