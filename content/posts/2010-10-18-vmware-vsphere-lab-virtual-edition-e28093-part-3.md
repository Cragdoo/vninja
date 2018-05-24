---
author: cmohn
comments: true
date: 2010-10-18 20:16:28+00:00
layout: post
link: http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%e2%80%93-part-3/
slug: vmware-vsphere-lab-virtual-edition-%e2%80%93-part-3
title: 'VMware vSphere Lab: Virtual Edition – Part 3'
wordpress_id: 432
categories:
- Howto
- Virtualization
tags:
- labs
- VMware
- VMware Workstation
- vSphere
---

![](/images/logos/vmware-logo.gif)This is the third post in a series outlining how to set up your own little virtualized **Virtual vSphere Lab**, if you missed [part one](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-1/) or [part two](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-2/), be sure to check them out first!

I'm going to assume that you already have VMware Workstation 7.x installed and ready. If not, get it installed before continuing.

Now that we have all the prerequisites in place, we're going to go straight to installing the first ESXi host for your lab.
<!-- more -->

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Prereqs-1a-300x122.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Prereqs-1a.png)


#### Installing ESXi in VMware Workstation 7.x


Fire up VMware Workstation, and select "New Virtual Machine", which brings up the "**Welcome to the New Virtual Machine Wizard**".

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-1-300x249.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-1.png)
[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-2-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-2.png)

Select the "**Typical**" setting and click on "**Next**". "**Select the "Installer disc image file (iso):**" option, click "**Browse**" and browse to the directory where you downloaded the ESXi ISO file. Select the "**VMware-VMisor-Installer-xxx-xxxxx-x86_64.iso**" file and click on "**Open**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-3-300x208.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-3.png)

This will return you to the "**New Virtual Machine Wizard**" screen, and it should have automatically detected that you have pointed it to an ESXi installer media.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-4-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-4.png)

Click on "**Next**" and the wizard will ask you to name the new virtual machine. Since this is the first ESXi instance I am installing, I decided to call it **lab-esxi1**, feel free to use your own naming conventions.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-5-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-5.png)

Next up is defining the disk capacity. ESXi has a minimum requirement of of **5 GB** disk space, according to the [ESXi Installable and vCenter Server Setup Guide pg. 24](http://www.vmware.com/pdf/vsphere4/r41/vsp_41_esxi_i_vc_setup_guide.pdf), but from experience it needs a lot less. I've set up this lab by using 2GB as the local disk size.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-6-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-6.png)

Click on "**Next**" again and you'll get a summary view of the settings you have done. Make sure the "**Power on this virtual machine after creation**" option is ticked, and click "**Finish**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-7-300x271.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-7.png)

The installer should now start automatically when the newly created VM is started.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-8-300x189.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-8.png) [![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-9-300x189.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-9.png)

It will stop on the welcome screen, where you need to hit "**Enter**" to make the installation start.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-10-300x189.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-10.png)
Hit "**Enter**" and then "**F11**" to accept the EULA.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-11-300x189.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-11.png)
Next you will be presented with the "**Select a Disk**" window, and since we only have the 2GB drive we added during the **New Virtual Machine wizard**, we'll just go with the default and click "**Enter**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-12-300x189.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-12.png) [![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-13-300x189.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-13.png)
Confirm the install by clicking "**F11**" and the installer will continue to actually install ESXi inside your new virtual machine.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-14-300x189.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-14.png) [![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-15-300x189.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-15.png)

When the installation finishes, you need to click "Enter" to make the virtual machine reboot and load ESXi for the first time. You should now disconnect the ISO file used to install it, since you need it available when you install your second host.

To do this, right click the "**lab-esxi1**" VM, and select settings.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-16.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-16.png)

Find "**CD/DVD (IDE)**" and clear the "**Connect at power on**" option.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-17-300x259.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-17.png)

Click "**Ok**" and you're returned to the freshly installed ESXi host.

Find the yellow box below your VM console output and click on "**I Finished Installing**" to inform VMware Workstation that the installation procedure is indeed completed.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-18-300x187.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-18.png) [![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-19-300x172.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-19.png)

That completes the installation of your first ESXi host, inside VMware Workstation. Now, to cut some corners, repeat the steps above but call the VM "**lab-esxi2**" and we'll have a two ESXi hosts ready for our lab environment.

![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-20-300x189.png)

In my lab environment, the whole installation of ESXi took a whopping 2 minutes and 41 seconds to complete, including going through the create a new VM wizard.

Next up, in [part 4](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-4/), installing a Windows Server 2008 R2 VM for VMware vCenter Server.
