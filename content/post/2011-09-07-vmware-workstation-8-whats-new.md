---
author: Christian Mohn
comments: true
date: 2011-09-07 12:07:23+00:00
layout: post
slug: vmware-workstation-8-whats-new
title: VMware Workstation 8 - What's new?
url: /virtualization/vmware-workstation-8-whats-new/
wordpress_id: 1415
categories:
- Virtualization
- VMware
tags:
- Beta
- Software
- VMware
- Workstation
- Workstation 8
---

<del>VMware is close (still in beta) to releasing the new major release of VMware Workstation.</del>

**Update 14. September 2011:** [VMware Workstation 8 has now officially been released.](http://www.vmware.com/products/workstation/)

VMware Workstation 8 brings a lot of new features and enhancements to the table, and I've been lucky enough to play around with it in the beta program.



### VMware Workstation 8 System Requirements


To be able to install VMware Workstation, the host system processor needs to meet the following requirements:


[![](http://vninja.net/wordpress/wp-content/uploads/2011/09/VMware-Workstation-8-Whats-New-CPU-Z-1-150x150.png)](http://vninja.net/wordpress/wp-content/uploads/2011/09/VMware-Workstation-8-Whats-New-CPU-Z-1.png)

  * 64-bit x86 CPU


  * LAHF/SAHF support in long mode


  

To determine if your host system is 64-bit capable, download [CPU-Z](http://www.cpuid.com/softwares/cpu-z.html) to determine the capabilities of your processor.

**To be able to run nested 64bit guests, like VMware vSphere 5 hosts, the system needs additional CPU features:**




  * AMD CPU that has segment-limit support in long mode.



  * Intel CPU that has VT-x support. VT-x support must be enabled in the host system BIOS.



For a list of Intel processors that support VT-x check [Intel® Virtualization Technology List](http://ark.intel.com/VTList.aspx), or use CPU-Z to identify that as well.



### VMware Workstation 8 - New features






  * **New User Interface**
The user interface has been updated with new menus, toolbars, and an improved preferences screen.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/09/VMware-Workstation-8-Whats-New-GUI-11-150x150.png)](http://vninja.net/wordpress/wp-content/uploads/2011/09/VMware-Workstation-8-Whats-New-GUI-11.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/09/VMware-Workstation-8-Whats-New-GUI-2-150x150.png)](http://vninja.net/wordpress/wp-content/uploads/2011/09/VMware-Workstation-8-Whats-New-GUI-2.png)

  



  * **Remote Connections**
Connect to Server feature allows remote connections to hosts running **Workstation**, **ESX 4.x** and **Virtual Center**.  You can now use Workstation as a single interface to access all of the VMs you need regardless of where they reside.



  



  * **Upload to vSphere**
Integrated vSphere drag and drop integration. Automatic usage of OVFTool enables easy uploading of VMs from VMware Workstation to ESX hosts or vCenter. Move workloads from local test environment into production environment with a few mouse clicks.



  



  * **Share your VMs**
This new features allows you to control who access them from other instances of Workstation, great feature for teams working together or single administrators that access the same VMs from multiple computers. Also, a VM that is shared is started with the host OS without starting the VMware Workstation GUI, similar to how VMware Server worked before it was discontinued.


  



  * **New default keyboard driver**
[![](http://vninja.net/wordpress/wp-content/uploads/2011/09/VMware-Workstation-8-Whats-New-Enhanced-Keyboard-Installation-1-150x150.png)](http://vninja.net/wordpress/wp-content/uploads/2011/09/VMware-Workstation-8-Whats-New-Enhanced-Keyboard-Installation-1.png)To limit the number of reboots required during installation/upgrade of VMware Workstation, the Enhanced Keyboard functionality is no longer installed by default. 
_Note: Upgrading from VMware Workstation 7 to 8 keeps and upgrades the existing driver unless VMware Workstation 7 is uninstalled before installing version 8._
  



  * **Virtual VT-x/EPT or AMD-V/RVI**
[![](http://vninja.net/wordpress/wp-content/uploads/2011/09/VMware-Workstation-8-Whats-New-VT-x-1-150x150.png)](http://vninja.net/wordpress/wp-content/uploads/2011/09/VMware-Workstation-8-Whats-New-VT-x-1.png)This is a good one, at least for all of us that run lab environments on our desktops or laptops. 
This setting enables you to run 64bit guests inside nested hypervisors like VMware vSphere 5. To enable it, edit the vCPU settings for the particular VM.
  



  * **Better Teams**

Add team attributes to any VM without any of the drawbacks.  No longer forced to make a Team in order to manage multiple VMs together.
  



  * **Improved graphics performance in guests**
  



  * **Improved vSMP**
  



  * **Other Virtual Hardware Improvements**
Memory support is now 64GB pr VM
HD Audio is available for Windows Vista, Windows 7, Windows 2008, and Windows 2008 R2 guests (RealTek ALC888 7.1 Channel High Definition Audio Codec)
USB 3.0 support for Linux guests. _(Not available for Windows guests)_
Bluetooth devices can be shared with Windows guests

