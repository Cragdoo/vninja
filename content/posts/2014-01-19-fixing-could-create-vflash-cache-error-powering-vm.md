---
author: cmohn
comments: true
date: 2014-01-19 19:19:02+00:00
layout: post
slug: fixing-could-create-vflash-cache-error-powering-vm
title: Fixing "Could not create vFlash cache" error when powering on a VM
url: /virtualization/fixing-could-create-vflash-cache-error-powering-vm/
wordpress_id: 3020
categories:
- Virtualization
tags:
- featured
- SSD
- vCenter
- vCenter Appliance
- VCSA
- vFlash Read Cache
- vFRC
- vSphere
- Web Client
---

During some way overdue housekeeping in my HP Microserver-based "Homelab" today, I ran into a rather annoying issue that prevented me from starting one of my more important VMs; namely my home Domain Controller.

In short, I removed an old SSD drive that I've used for vFlash Read Cache (vFRC) testing and installed a new 1TB drive instead. Since I have a rather beefy work lab now, I need space more than speed at home, so this seemed like a good idea at the time.

A good idea that is, until I tried starting my DC VM, which also is my DNS and DHCP server, and got greeted with this little gem:

[![Zappa-SSD-Failure](http://vninja.net/wordpress/wp-content/uploads/2014/01/Zappa-SSD-Failure.png)](http://vninja.net/wordpress/wp-content/uploads/2014/01/Zappa-SSD-Failure.png)

The "_Could not create vFlash cache: msg.vflashcache.error.VFC_FAILURE_" error is understandable, the SSD drive was removed, but I honestly did not think that even if a VM was configured to use it, that it's absence would prevent a power-on operation on that VM. I would have expected it to throw a warning about the cache location missing, and warn me that acceleration was not happening, not a flat out "_cannot power on_".

Normally the fix for this would be quick and easy, edit the VM and remove the **vFRC** configuration, but since my host has a whopping total of 8GB of memory I don't have vCenter running at all times.  Editing the VM settings through the vCenter Legacy Client (C#) does not work, since vFRC requires Virtual Hardware Version 10, which it cannot edit.

Once I got the vCenter Server Appliance (vCSA) fired up, I realised that I have somehow forgotten the admin AND root passwords, and was completely unable to log-on. How that has happened is beyond me, but for the life of me I was not able to log on.

Next step was to try and edit the VM from VMware Workstation 10 installed on one of the Windows boxes in my network. Sadly Workstation has no concept of **vFRC**, and it is not possible to edit that particular VM setting, even if you connect it to the vSphere host. I later on also realized that VMware Workstation 10 is also unable to host connect  USB peripherals, like printers, to a VM, but that's beside the point right now.[**_
_**](https://www.google.com/search?client=safari&rls=en&q=peripherals&spell=1&sa=X&ei=PCHcUtu7KoeS5ASH1IGoDw&ved=0CCcQvwUoAA)

So, either trying to hack the VM Hardware Version to a value that the vSphere Client can handle, of  deploying a new **vCSA** instance, I was left with editing the VMs _vmx_ file directly.

Thankfully this was an easy way to fix the problem, and get the VM powered on. For each vmdk file that is configured to use **vFRC**, there is a corresponding entry in the _vmx_ file, that controls **vFRC**.

In order to turn off **vFRC** acceleration for a given disk, download the vmx file, and change the value for <device>.vFlash.enabled from
[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
sched.scsi0:2.vFlash.enabled = "TRUE"
[/cc]
to
[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
sched.scsi0:2.vFlash.enabled = "FALSE"
[/cc]

Re-upload the _vmx_ file, and try to power on. In my case, this fixed the problem of powering on the VM.

This problem again highlights one of the problems with the dependancy on the new vSphere Web Client. In a real production environment, the vCenter would be up and running at all times, and editing the VM would have been a small task, but what if you had used **vFRC** to speed up **vCSA** itself, and you had a failed SSD drive?

Of course, in this case the fault is mostly mine. I removed a "production SSD", without first removing the **vFRC** configuration. I did not have a working vCSA with Web Client up and running when this was done, and I had forgotten my passwords. A pretty specific error generating procedure if there ever was one.

It's an easy fix to edit the _vmx_ file, but it does at times feel a little bit like you are now able to cut of the branch you are sitting on with the new vSphere Web Client. In simpler days, you could pretty much fix anything by connecting the vSphere Client to the host and fix any errors there, but now the dependancy on the vCenter Server is stronger than ever.

_ Before upgrading your VMs to Hardware Version 10, make sure you understand the implications of going all-in with dependancies on the Web Client and vCenter. It might just come back and bite you if you __haven't thought through your design._
