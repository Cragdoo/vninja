---
author: Christian Mohn
comments: true
date: 2012-07-02 19:50:03+00:00
layout: post
slug: vsphere-5-creating-hardware-version-4-vm
title: 'vSphere 5: Creating a Hardware Version 4 VM'
url: /virtualization/vsphere-5-creating-hardware-version-4-vm/
wordpress_id: 1888
categories:
- Virtualization
tags:
- EasyVNX
- ESX
- ESX 3.5
- ESXi
- Hardware Version
- VMware
- vSphere
- workaround
---

I recently had a need to create a VM for usage on ESX 3.x, but the only thing available to me in my lab was vSphere 5, which naturally creates new VMs with the latest and greatest version 8 virtual hardware.

The following table lists the virtual hardware versions and the corresponding ESX and ESXi releases: 
<table cellpadding="0" cellspacing="0" >
<tbody >
<tr >

<td >**Version**
</td>

<td >**Virtual Hardware Version**
</td>
</tr>
<tr >

<td >ESXi 5.x
</td>

<td >8
</td>
</tr>
<tr >

<td >ESX/ESXi 4.x
</td>

<td >7
</td>
</tr>
<tr >

<td >ESX 3.x
</td>

<td >4
</td>
</tr>
<tr >

<td >ESX 2.x
</td>

<td >3
</td>
</tr>
</tbody>
</table>
Naturally, an ESXi 5.x host can run VMs created with older hardware versions but it is not possible (through the vSphere Client) to create a new VM with an old hardware version, and I needed to create a v4 VM for later export to an ESX 3.x host. 

Of course, I could have created the VM on the ESX host that it was supposed to run on on the first place, but the host in question was unavailable at the time and the plam was to prepare the VM off site before deployment in the production environment.

Since I did not have a valid license for ESX 3.5 any more, the easiest way I found to create a hardware version 4 VM, and configure it on vSphere 5, was to use [Easy VMX](http://www.easyvmx.com/) to create the basic VM (vmx file) instead of hacking away at creating it manually.

Easy VMX created the .VMX file that I needed to register a hardware version 4 VM to a ESXi 5.x host, and edit it through the vCenter client. Of course, it creates disk files that are incompatible with ESX but that´s of no concern as you can just remove the invalid devices and add new ones through the vCenter client.

All in all this little "trick" worked without a hitch, and the small Linux machine I had set up for the client worked perfectly when deployed on the old ESX 3.5 install.

Of course, the best option would be to reinstall the old ESX 3.5 host with ESXi 5, but as this was a system due to be decommissioned very soon that was not a real option for the client. The main purpose of the VM I created was to act as an alerting system for incoming email, warning that the email addresses had changed to a new domain, and emails sent to the old one would not be read or acted upon.
