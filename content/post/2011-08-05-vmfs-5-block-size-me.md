---
author: Christian Mohn
comments: true
date: 2011-08-05 12:21:31+00:00
layout: post
slug: vmfs-5-block-size-me
title: 'VMFS-5: Block Size Me'
url: /virtualization/vmfs-5-block-size-me/
wordpress_id: 1365
categories:
- Virtualization
- VMware
tags:
- Block Size
- Datastore
- storage
- VMFS
- VMware
- vSphere 5
---

The up and coming release of [VMware vSphere 5](http://www.vmware.com/company/news/releases/vmw-cloud-infrastructure-071211.html) comes with an upgraded versjon of the VMware vStorage VMFS volume file system. One of the problems with VMFS-3 an earlier is that the block size you define when you format the datastore, determines the maximum size of the VMDK files stored on it. This means that when planning your datastore infrastructure you must have an idea on how large your VMDK files will potentially be during the lifecycle of the datastore.

For **VMFS-2** and **VMFS-3** the block sizes  and their impact on VMDK files looks as follows:

[table id=2 /]

In other words, if you format your datastore with a 1MB block size, with **VMFS-3**, you are limited to a maximum VMDK file size of 256GB. Of course, you can work around this by adding more VMDK files to your VM, and then extending the disks inside the installed OS in the VM, but over time that might get a bit messy. The only way to change the block size, is to migrate all the VMs stored on that particular datastore to a different one, and reformat your original datastore with a new block size. For environments with limited storage space, this can be a real headache.

Thankfully VMware has made this a thing of the past in VMware vSphere 5, and VMFS-5. VMFS-5 has a  new unified block size of 1MB, and which _no longer_ limits you to 256GB VMDK files.  In fact, the block size no longer really matters, as the limits are removed completely.

The table for vSphere 5 and VMFS-5 looks a bit simpler:

[table id=3 /]

Upgrading from VMFS-3 to VMFS-5 is an **online & non-disruptive** upgrade operation, meaning your can do it while your VMs are running on the datastore.

Thankfully you can also [extend VMDK files, to the new limits, on an upgraded VMFS-3 datastore](http://blogs.vmware.com/vsphere/2011/08/2tb-vmdks-on-upgraded-vmfs-3-to-vmfs-5-really.html), given that it has been upgraded to VMFS-5.

**Note: Remember to update all your hosts to vSphere 5 before upgrading your datastores, since vSphere 4 (and earlier) can't read the new VMFS-5 filesystem.**

This is great news for vAdmins, since we no longer have to worry about the block size as a limiting factor for our VMs._ Simplification is always welcome!_

Of course, there are other improvements in VMFS-5 as well, but we'll save those for a later post or two.
