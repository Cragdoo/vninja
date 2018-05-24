---
author: cmohn
comments: true
date: 2010-10-18 07:00:55+00:00
layout: post
link: http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-2/
slug: vmware-vsphere-lab-virtual-edition-part-2
title: 'VMware vSphere Lab: Virtual Edition - Part 2'
wordpress_id: 415
categories:
- Howto
- Virtualization
tags:
- labs
- VMware
- VMware Workstation
- vSphere
---

![](/images/logos/vmware-logo.gif)This is the second post in a series outlining how to set up your own little virtualized **Virtual vSphere Lab**, if you missed [the first one](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-1/) be sure to check it out!

The first step in setting up the **The Virtual vSphere Lab**, is making sure you have everything you need. To get everything up and running, you’ll need the following:

<!-- more -->




	
  1. **VMware Workstation 7.x downloaded and installed.**
A 30 day trial version download is available at [vmware.com](http://www.vmware.com/products/workstation/) (registration required)

	
  2. **Installation files for VMware vSphere Hypervisor (ESXi)**
You can download it directly from [vmware.com](http://www.vmware.com/products/vsphere-hypervisor/index.html) (registration required)
Download the **ESXi 4.1 Installable (CD ISO)** (current version as of writing) and store it locally.


  3. **VMware vCenter Server 4.1 and modules**
Download from [vmware.com](http://downloads.vmware.com/d/info/datacenter_downloads/vmware_vsphere_4/4) (registration required)
	
  4. **Windows Server 2008 R2 installable media.**
You can download a evaluation 180 day version from [microsoft.com](http://technet.microsoft.com/en-us/evalcenter/dd459137.aspx).
Be sure to download the 64-bit compilation edition ISO file and store it locally.

	
  5. **Shared storage solution**
There are a number of options available to you, but the most common ones are probably
[Openfiler](http://www.openfiler.com/) free and offers both iSCSI and NFS connectivity
[FreeNAS](http://freenas.org/doku.php) free and offers both iSCSI and NFS connectivity
[StorMagic SvSAN](http://www.stormagic.com/SvSAN.php) offers up to 2 TB of iSCSI storage in the free version, and plugs directly into vCenter.
[Lefthand VSA](http://h18000.www1.hp.com/products/storage/software/vsa/index.html) the HP StorageWorks P4000 Virtual SAN Appliance comes as a 60 day free trial offers both iSCSi and NFS.
[Celerra UBER v3.2](http://nickapedia.com/2010/10/04/play-it-again-sam-celerra-uber-v3-2/) comes as either a VMware Workstation or vSphere version, preconfigured as a virtual machine.


This virtualized version of the [EMC Celerra](http://www.emc.com/products/family/celerra-family.htm), built by[ Nick Weaver](http://nickapedia.com/) ([@lynxbat](http://twitter.com/lynxbat)) is my choice for this series of posts. Mostly because I’ve played with most of the other options before, but also because the newest EMC Celerra offerings looks really interesting and I want to become acquainted with the management of it.

So, get your downloads on and be ready to follow [part 3](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-3/) where I'll walk you through the initial setup and configuration of your own **Virtual vSphere Lab**.
