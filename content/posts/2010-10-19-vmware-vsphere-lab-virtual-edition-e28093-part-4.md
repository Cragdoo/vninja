---
author: cmohn
comments: true
date: 2010-10-19 18:01:49+00:00
layout: post
slug: vmware-vsphere-lab-virtual-edition-%e2%80%93-part-4
title: 'VMware vSphere Lab: Virtual Edition – Part 4'
url: /virtualization/vmware-vsphere-lab-virtual-edition-%e2%80%93-part-4/
wordpress_id: 487
categories:
- Howto
- Virtualization
tags:
- labs
- VMware
- VMware Workstation
---

![](/images/logos/vmware-logo.gif)This is the fourth post in a series outlining how to set up your own little virtualized **Virtual vSphere Lab**, if you missed [part one](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-1/), [part two](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-2/), or [part three](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-3/) be sure to check them out first!

In part 4 we'll be installing a Windows Server 2008 R2 VM, to use as the basis for the VMware vCenter Server installation.

First off, create a new VM in VMware Workstation. This works exactly the same way as we did it in [part 3](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-3/), but I'll repeat the steps here as well.
<!-- more -->
Select “**New Virtual Machine**”, which brings up the “**Welcome to the New Virtual Machine Wizard**“.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-1-300x189.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-1.png) [![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-21-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-21.png)


Select the "**Typical**" setting and click on "**Next**". "**Select the "Installer disc image file (iso):**" option, click "**Browse**" and browse to the directory where you downloaded the Windows Server 2008 R2 ISO file. 

Select the "**7600.16385.090713-1255_x64fre_server_eval_en-us-GRMSXEVAL_EN_DVD.iso**" file and click on "**Open**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-3-300x208.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-3.png)

This will return you to the “**New Virtual Machine Wizard**” screen, and it should have automatically detected that you have pointed it to an Windows Server 2008 R2 installer media.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-4-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-4.png)

Click on “**Next**” and the wizard will ask you to fill out the required Windows Server details. Since we're using a trial version, I left the "Windows product key" empty. I elected to use "Windows Server 2008 R2 Enterprise" as the version to install. Fill out a password (optional) and click "**Next**". Clicking "Next" brings up a warning regarding the Windows product key. Since we don't have one at this point, click on "**Yes**" to continue.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-5-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-5.png) 
[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-6-300x96.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-6.png)

Give the new virtual machine name, and since this is the server that will be used as the vCenter Server, I decided to call it _lab-vc1_, feel free to use your own naming conventions, of course.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-7-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-7.png)

Click "**Next**" to continue. The "**Specify Disk Capacity**" dialogue pops up, asking you how to size the VM. I went with the default of 40GB. By default all VMs created in VMware Workstation are so-called thin provisioned. This means that the virtual disk files are only as big as the actual data in contains, and that it grows towards the limit you set during setup. 

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-8-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-8.png)
Again, click "**Next**" and review the summary. 

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-9-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-9.png)

Since I'm not doing anything special with this VM at this time, hit "**Finish**" and let the installer start.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-10-300x231.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-10.png)

VMware Workstation 7.x is pretty clever, and since it detected that we're installing a Windows Server 2008 R2 machine, the actual Windows installation runs unattended, without the need for manual intervention.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-11-300x233.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-11.png) [![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-12-300x233.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-12.png) [![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-13-300x233.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-13.png) 

Note that even if you do get the "**Press CTRL-ALT-DELETE**" to log on prompt, the install still isn't finished. Notice the little yellow area below the VM console window:

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-14-300x20.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-14.png)

As long as that is present, leave the VM alone as it's still installing in the background. 

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-15-300x218.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-15.png)

After a while, the installation is finished and we're ready to log on. Log on with the username and password specified in the wizard.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-16-300x218.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-16.png)

After logging on, have a look at the "**Initial Configuration Tasks**" window and click on "**Provide computer name and domain**".

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-17-300x90.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-17.png)

Click on "**Change**" in the "**System Properties**" window

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-18-267x300.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-18.png)

Enter your server name in the "**Computer name:**" box, I used lab-vc1 and click on "**OK**".

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-19-255x300.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-19.png)

Click on the "OK" button to restart the computer

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-20-300x127.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-20.png)

That's it for now. 

In the next part, part five,  I'll cover the installation of VMware vCenter Server and connecting it to the ESXi hosts.



