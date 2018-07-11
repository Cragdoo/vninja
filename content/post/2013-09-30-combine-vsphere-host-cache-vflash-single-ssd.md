---
author: Christian Mohn
comments: true
date: 2013-09-30 00:12:39+00:00
layout: post
slug: combine-vsphere-host-cache-vflash-single-ssd
title: Can you combine vSphere Host Cache and vFlash on a single SSD?
url: /vmware-2/combine-vsphere-host-cache-vflash-single-ssd/
wordpress_id: 2750
categories:
- VMware
tags:
- '5.5'
- ESXi
- featured
- Host Cache
- vCenter
- vFlash
- Virtualization
- VMware
- vSphere
---

One of the new features in vSphere 5.5 is the vSphere vFlash that enables you to use a SSD/Flash device as a read cache for your storage. Duncan Epping has a series of posts on [vSphere Flash Cache](http://www.yellow-bricks.com/2013/08/26/introduction-to-vsphere-flash-read-cache-aka-vflash/) that is well worth a read.

vSphere vFlash caches your read IOs, but at the same time you can use it as a swap device if you run into memory contention issues. The vSphere vFlash Host Cache is similar to the older [Host Cache feature](http://vninja.net/vmware-2/testing-vmware-vsphere-5-swap-to-host-cache/), but if you are upgrading from an older version of ESXi there is a couple of things that needs to be done to be able to use this feature. [![Screen Shot 2013-09-30 at 02.19.57](http://vninja.net/wordpress/wp-content/uploads/2013/09/Screen-Shot-2013-09-30-at-02.19.57-150x150.png)](http://vninja.net/wordpress/wp-content/uploads/2013/09/Screen-Shot-2013-09-30-at-02.19.57.png)

**If you had the "old" Host Cache enabled before upgrading to v5.5, you have to delete the dedicated Host Cache datastore and re-create a new vSphere vFlash resource to be able to use both vFlash Host Cache and vSphere Flash Read Cache on the same SSD/Flash device.**

Also note that vFlash Read Cache is only available for VMs that run in ESXi 5.5 Compatibility Mode aka Virtual Hardware Version 10, and is enabled pr. VMDK in the VMs settings.

[![Screen Shot 2013-09-30 at 02.19.57](http://vninja.net/wordpress/wp-content/uploads/2013/09/Screen-Shot-2013-09-30-at-02.19.57-150x150.png)](http://vninja.net/wordpress/wp-content/uploads/2013/09/Screen-Shot-2013-09-30-at-02.19.57.png)Now you can utilize vFlash to both accelerate your read IOs, and speed up your host if you run into swapping issues. Good deal!
