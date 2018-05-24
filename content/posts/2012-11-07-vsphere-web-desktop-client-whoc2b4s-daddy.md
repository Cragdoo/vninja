---
author: cmohn
comments: true
date: 2012-11-07 14:33:52+00:00
layout: post
link: http://vninja.net/vmware-2/vsphere-web-desktop-client-who%c2%b4s-daddy/
slug: vsphere-web-desktop-client-who%c2%b4s-daddy
title: vSphere Web or Desktop Client - Who´s Your Daddy?
wordpress_id: 2185
categories:
- VMware
tags:
- management
- Virtualization
- VMware
- vSphere
- vSphere 5
- Web Client
---

At the moment, VMware vSphere offers two different management clients, the vSphere Web Client and the vSphere Desktop Client.

The feature comparison table looks like this:
(Copied from "[Which vSphere client should I use and when?](http://blogs.vmware.com/vsphere/2012/11/which-vsphere-client-should-i-use-and-when.html)")
<table cellpadding="0" width="670" cellspacing="0" border="0" >
<tbody >
<tr >

<td width="266" valign="top" >**vSphere Web Client Only**
</td>

<td width="266" valign="top" >**vSphere Desktop Client Only**
</td>
</tr>
<tr >

<td width="266" valign="top" >



	
  * vCenter Single Sign-On

	
    * Authentication

	
    * Administration




	
  * Navigation with Inventory Lists

	
  * Inventory Tagging

	
  * Work In Progress (Pause)

	
  * Pre-emptive Searching

	
  * Save Searches

	
  * Enhanced read performance utilizing the Inventory Service

	
  * vSphere Replication (not SRM)

	
  * Virtual Infrastructure Navigator

	
  * Enhanced vMotion (no shared storage)

	
  * Integration with vCenter Orchestrator (vCO) Workflows (Extended Menus)

	
  * Virtual Distributed Switch (vDS)

	
    * Health Check

	
    * Export/Restore Configuration

	
    * Diagram filtering




	
  * Log Browser Plugin

	
  * vSphere Data Protection (VDP)



</td>

<td width="266" valign="top" >



	
  * VMware Desktop Plug-ins (VUM, SRM, etc)

	
  * 3rd Party Desktop Plugins (various)

	
  * VXLAN Networking

	
  * Ability to change Guest OS on an existing virtual machine

	
  * vCenter Server Maps

	
  * Create and edit custom attributes

	
  * Connect direct to a vSphere host

	
  * Inflate thin disk option found in the Datastore Browser


 ;
</td>
</tr>
</tbody>
</table>
**In plain words, this means that all new features in vSphere 5.1 are Web Client _only_, and older "legacy" features and plugin integrations are Desktop Client only at this point.**

This will change over time as both VMware products, like VUM and SRM, are moved into a new home in the Web Client and when 3rd party vendors integrate their plugins with the new client.

There is no doubt that the vSphere Web Client is where the future lives, but in the interim vAdmins are forced to utilize _both_ to be able to use all the available functionality and obviously this is far from ideal.

I´m sure VMware will get where they want with the vSphere Web Client in the end, and changing platform like this is a big task, especially when you consider that third parties need to be on their toes and upgrade their integrations as well.

Having two clients for management is not fun, but it does beat having [no management at all](http://searchservervirtualization.techtarget.com/news/2240160813/Hyper-V-30-tools-wont-emerge-until-Windows-Server-2012-SP1).

So in short, your daddy? They both are. While they might be separated, the divorce has not been finalized just yet.
