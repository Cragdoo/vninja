---
author: cmohn
comments: true
date: 2010-08-24 21:48:02+00:00
layout: post
slug: vcenter-update-manager-to-lose-its-fat
title: vCenter Update Manager to lose it's fat
url: /virtualization/vcenter-update-manager-to-lose-its-fat/
wordpress_id: 176
categories:
- Virtualization
tags:
- Ops
- vCenter
- vCenter Update Manager
- Virtualization
- VMware Player
- VMware Workstation
---

![](/images/logos/vmware-logo.gif)[Dwayne Lessner](http://twitter.com/dlink7) who runs [IT Blood Pressure](http://www.itbloodpressure.com/), has written a guest post on GestaltIT called [Is My Favourite VSphere Tool Is Going Away?](http://gestaltit.com/all/tech/storage/guest/favourite-vsphere-tool/) 

In his article, Dwayne talks about vCenter Update Manager 4.1, and the fact that it seems to be the last version of the tools that will allow you to patch your Windows and Linux guests:



<blockquote>
VMware vCenter Update Manager Features. vCenter Update Manager 4.1 and its subesquent update releases are the last releases to support scanning and remediation of patches for Windows and Linux guest operating systems and applications running inside a virtual machine. The ability to perform virtual machine operations such as upgrade of VMware Tools and virtual machine hardware will continue to be supported and enhanced.  

[VMware vSphere 4.1 release notes](http://www.vmware.com/support/vsphere4/doc/vsp_esx41_vc41_rel_notes.html#featureplatformnotice)
</blockquote>



Dwayne talks about this as being _a bad thing_, and that's where I disagree. I have **never** understood why VMware saw it as their job to patch the operating systems the guests are running, and I have yet to see anyone actually use this feature. Obviously I was wrong, someone does indeed use it, but I really can't understand why.

I'm a keen believer in doing what you know, and doing it well. Let "native" patching solutions take care of the guests, [Windows Server Update Services (WSUS)](http://technet.microsoft.com/en-us/wsus/default.aspx) comes to mind, and leave vCenter Update Manager (VUM) to take care of patching your VMware products.

I wouldn't mind seeing vCenter Update Manager (VUM) extended into patching the VMware Workstation, Fusion and Player installations your enterprise might have, but I really think that losing the fat that is guest OS patching can only be a good thing.
