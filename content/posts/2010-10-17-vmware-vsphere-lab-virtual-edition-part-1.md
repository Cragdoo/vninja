---
author: cmohn
comments: true
date: 2010-10-17 20:16:44+00:00
layout: post
link: http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-1/
slug: vmware-vsphere-lab-virtual-edition-part-1
title: 'VMware vSphere Lab: Virtual Edition - Part 1'
wordpress_id: 154
categories:
- Howto
- Virtualization
tags:
- labs
- VMware
- VMware Workstation
- vSphere
---

![](/images/logos/vmware-logo.gif)Even if VMware vSphere has pretty stringent [hardware requirements](http://www.vmware.com/resources/compatibility/search.php), there are some rather cheap options available if you want to set up your own vSphere Lab for training or testing purposes.

In this first post of the series, I'll examine the option of setting up a Virtual vSphere Lab, based on [VMware Workstation 7.1](http://www.vmware.com/products/workstation/):


### The Virtual vSphere Lab


In VMware Workstation 7.1, running [vSphere 4.1 is supported](http://www.vmware.com/support/ws71/doc/releasenotes_ws711.html#whatsnew) as a guest operating system. This means that you can run your whole test environment as virtual instances on your laptop or desktop computer, not worrying about the vSphere HCL at all.
<!-- more -->
Initially this might sound like a great way to set up a testing/training lab, but there are some caveats that you will need to consider;



	
  * **Processor requirements**
Running ESX in VMware Workstation requires that your host CPU supports Intel EM64T with VT-x or AMD64 Family 10H and later processors with AMD-V.

	
  * **Nested virtual machines can be 32bit only**
This basically means that you can not run Windows Server 2008 R2 as a nested guest (64bit only), nor will you be able to install vCenter 4.1. You can, of course, run your vCenter test alongside the virtualized ESX guests, just not nested inside them.

	
  * **Performance**
Performance will not be comparable with running ESX on bare-metal. Also take into consideration how many VMs you are going to run, as this will have great impact on the overall performance.




#### Example Usage


Lets assume that you want to test vMotion between ESX hosts. This would require, at least, **5 VMs** running in your virtualized lab environment:



	
  * two ESX guests to vMotion the VM between

	
  * one vCenter guest

	
  * one guest providing shared storage

	
  * at least VM running nested inside ESX to do the actual vMotion


All of these would probably be stored on the same storage, most likely a local SATA drive in your desktop or laptop computer. In addition, this setup requires a minimum of at least 5-6 GB RAM as well, just to get it running, and that's in addition to the memory required to run the Windows or Linux installation that VMware Workstation runs on.


#### Conclusion


A virtual vSphere lab is a quick and easy way to test features and play around. For proof of concept and familiarization with the vSphere products it might fit the bill, but don't expect to get much performance testing out of it.

[In part 2](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-2/) of the series, I'll walk you through getting hold of the requirements you need to have in place before installing and configuring your virtual lab environment.
