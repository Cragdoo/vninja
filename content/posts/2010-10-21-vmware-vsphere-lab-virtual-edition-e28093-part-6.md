---
author: cmohn
comments: true
date: 2010-10-21 21:10:47+00:00
layout: post
slug: vmware-vsphere-lab-virtual-edition-%e2%80%93-part-6
title: 'VMware vSphere Lab: Virtual Edition – Part 6'
url: /virtualization/vmware-vsphere-lab-virtual-edition-%e2%80%93-part-6/
wordpress_id: 610
categories:
- Howto
- Virtualization
tags:
- labs
- vCenter
- VMware
- VMware Workstation
- vSphere
---

![](/images/logos/vmware-logo.gif)This is the sixth post in a series outlining how to set up your own little virtualized **Virtual vSphere Lab**, if you missed [part one](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-1/), [part two](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-2/), [part three](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-3/), [part four](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-4/),  or [part five](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-5/) be sure to check them out first!

Now that we have working DHCP and DNS services in our lab network, we're ready to get out two little ESXi friends connected to it's management service, VMware vCenter Server. 

To be able to do that, we obviously need to install vCenter Server. Here it goes!



#### Installing VMware vCenter Server



Installing VMware vCenter Server is a pretty straight forward task. 
First off, connect the **VMware-VIMSetup-all-4.1.0-259021.iso** file to the _srv-vc1_ server.
<!-- more -->

To do this, right click _srv-vc1_ and select "**Settings...**". 
In the "**Virtual Machine Settings**" window find "**CD/DVD (IDE)**" and select "**Use ISO image file:**". Then browse to your download location and find **VMware-VIMSetup-all-4.1.0-259021.iso** 

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-1-300x260.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-1.png)
Click on "**OK**" and the Windows AutoPlay feature shut prompt you what to do with the newly inserted DVD. Select "**Run autorun.exe**". 

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-2-300x286.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-2.png)
If you don't get the _AutoPlay_ window browse to the DVD in Windows Explorer and run the DVD from there. Select "**vCenter Server**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-3-300x232.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-3.png)
Now, **Windows User Account Control** pops up, and since we know what we're doing, we'll just click on "**Yes**" and continue.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-4-300x173.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-4.png)

Next up, select which language to use for the installation. Since I don't speak, or read, any of the other languages "**English**" is my obvious choice.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-5-300x112.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-5.png)

Click "**OK**" and the installation starts.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-6-300x143.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-6.png)
[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-7-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-7.png)

Once again, click "**Next**", read the "**End-User Patent Agreement**" thoroughly and click on "**Next**" again to continue. 

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-8-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-8.png)

Read the "**License Agreement**" before you click on "**I agree to the terms in the license agreement**" then click on "**Next**" to continue

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-9-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-9.png)

Now you need to fill our the "**Customer Information**" screen. 
Notice that if you don't provide a license key, it will run in evaluation mode, which is what we want for our lab.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-10-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-10.png)
click "**Next**" again to continue to the "**Database Options**" setup screen. The defaults are to install a local Microsoft SQL Server 2005 Enterprise instance for this vCenter installation, and in our little lab environment that's the perfect option.  
Leave the defaults as they are, and click on "**Next**" . The next screen is where you configure which user account the vCenter Server Service runs as, and again accept the defaults and continue by clicking "**Next**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-11-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-11.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-12-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-12.png)

The default destination folder is fine too, so we'll quickly click on "**Next**" to get going

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-13-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-13.png)

Next up is the "**vCenter Linked Mode Options**" window, and yet again the default of "**Create a standalone VMware vCenter Server instance**" is exactly what this lab requires, so we'll click on "**Next**" again

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-14-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-14.png)

In the "**Configure Ports**" window, we'll leave the defaults as is as well. 
There is no need to make things complicated by differing from the standard setup.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-15-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-15.png)

"**Next**" again, which brings us to the "vCenter Server JVM memory" screen. 
Since I seriously doubt we will be running more than 100 hosts in our lab environment, the default of "Small (Less than 100 hosts 1024 MB) will do just fine.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-16-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-16.png)

After another exciting "**Next**" click, we're finally ready to start the actual installation.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-16-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-16.png)

Click "**Install**" and the installation starts and runs without further interaction needed until it's finished.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-18-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-18.png)
[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-19-300x273.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-19.png)
[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-21-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-21.png)
[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-22-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-22.png)

Click on "**Finish**" and your brand new vCenter Server is installed! 
Even if the vCenter Server installation is indeed finished, you still need to do at least one more step before you can start using it properly. 
You need to get the VMware vSphere Client client installed, to be able to connect to the vCenter Server and start configuring it.



#### Installing the VMware vSphere Client


There are several ways to get hold of the VMware vSphere Client installation files. You can open your browser and browse to the IP address of one of the ESXi hosts and download it from there or you can install it from the same DVD that you installed vCenter Server from.

I chose to do the latter, and install it from the DVD. To install it from the dvd, open Windows Explorer, and double click the installation DVD. 
This will bring up the initial installer screen again, but this time we will select vSphere Client

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-23-300x232.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-23.png)

Once again the Windows "**User Account Control**" pops up, and we'll click on "**Yes**" to continue. 
Since this install is your basic "**Next**" "**Next** "**Next**" installation, I'll just post the screenshots of the procedure. Accept all defaults, and you should be ready to go.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-24-300x111.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-24.png)
[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-25-300x143.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-25.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-26-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-26.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-27-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-27.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-28-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-28.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-29-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-29.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-30-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-30.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-31-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-31.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-32-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-32.png)

Now the the vSphere Client is installed, you get a new icon on your desktop called "**VMware vSphere Client**", double click it to start the client.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-34-300x265.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-34.png)

Fill out _lab-vc1_ in the "**IP address / Name**" field and tick the "**Use Windows session credentials**" option. Click on "**Login**" to log into your fresh vCenter installation.

You'll now get a security warning that tells you that the current SSL certificate installed on the vCenter is untrusted. 
This is because vCenter creates a self-signed certificate during installation. As this is in a lab environment, that's fine. 
Tick the "**Install this certificate and do not display any security warnings for "lab-vc1"**." option and click on "**Ignore**".

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-35-300x125.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-35.png)

This will save the certificate from the vCenter Server in your vCenter Client, and suppress the SSL certificate warnings for further sessions.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-36-300x194.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-36.png)

Finally you get logged in, and presented with a warning that since you are running an unlicensed vCenter installation and that it will stop working after 60 days. Again, this is our lab, we don't care and click "**OK**".

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-37-300x209.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/VMware-vSphere-Lab-Virtual-Edition-–-Part-6-Installing-vCenter-37.png)


And there it is, both vCenter Server and vCenter Client installed and ready for use! In part seven, I'll cover connecting the vCenter Server and the lab ESXi hosts.
