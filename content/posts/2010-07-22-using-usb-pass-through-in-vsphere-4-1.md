---
author: cmohn
comments: true
date: 2010-07-22 23:03:27+00:00
layout: post
slug: using-usb-pass-through-in-vsphere-4-1
title: Using USB Pass-through in vSphere 4.1
url: /virtualization/using-usb-pass-through-in-vsphere-4-1/
wordpress_id: 131
categories:
- Howto
- Virtualization
tags:
- ESX
- Ops
- USB
- vCenter
- Veeam
- VMware
- vSphere
---

![](/images/logos/vmware-logo.gif)
Finally USB pass-through is possible on ESX hosts with the new vSphere 4.1 release! This feature ha been available in VMware Workstation/Fusion and Player for quite a while. The freshly added feature in vSphere 4.1 even works if you vMotion the guest from one host to another, which is in itself pretty amazing functionality!

In this post, I'll show how to setup and use the new USB pass-through feature in vSphere 4.1.



#### Setting up USB pass-through in vSphere 4.1



First off, we need to add an USB controller to the VM we want the USB pass-through working on. This is done by firing up the vSphere Center Client and right-clicking the VM. Then select **Edit Settings**

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-1-300x200.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-1.png)

Click on **Add** and find USB Controller from the list then click _Next_

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-2-300x285.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-2.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-3-300x241.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-3.png)

Click **Next** and you'll be presented with a list of the currently host-connected available USB devices. If none show up, make sure it's actually connected to the host. If your device is indeed connected, but still not listed in the vSphere Center Client it's not supported.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-4-300x235.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-4.png)

In my test setup I have a small APC UPS connected to the host, so I'll add that to the VM. Also note that this is where you enable vMotion support! Find your device, and click on **Next**

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-5-300x235.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-5.png)

Review your changes and click on **Finish**

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-6-300x235.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-6.png)

This will return you to the **Edit Settings** window. Click on **Ok** to have the USB controller and device(s) added to your VM. 

Connect to your VM and install the drivers, if needed, and you should be able to use your USB device directly inside the VM.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-7-300x256.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Using-USB-Pass-through-in-vSphere-4.1-7.png)



#### Usage Scenarios


What could you possibly use this new feature to accomplish? Well, for one you could use it to connect your UPS to your management software, without having to install any management software on the host itself. In general I would recommend using UPS vendors that offer direct vCenter integration instead, but for a lab environment this should work out nicely.

Another obvious usage pattern would be to connect USB dongles that some software require, either for security or for licensing purposes.

The one thing that springs to my mind, and one that would probably be the most useful in my environment, is to connect USB HDDs to the host and use those as a backup target for [Veeam Backup and Recovery](http://www.veeam.com/vmware-esx-backup.html). Being able to directly connect some cheap storage to the host and then connecting it directly into the Veeam Backup and Recovery VM makes it easy to backup/replicate your VMs for manual off-site storage. [Kendrick Coleman](http://twitter.com/KendrickColeman) ([kendrickcoleman.com](http://www.kendrickcoleman.com/)) had the same idea, but unlike him I'll try to make sure my [HDDs are located off-site before the fire starts](http://www.veeam.com/forums/viewtopic.php?f=2&t=4789&start=0&hilit=usb)! :-)

I'm sure that there are other usage scenarios as well, like connecting scanners, cameras and whatnot, I'm just not sure I'd like all sorts of devices connected to my hosts.
