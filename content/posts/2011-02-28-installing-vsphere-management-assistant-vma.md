---
author: cmohn
comments: true
date: 2011-02-28 23:26:15+00:00
layout: post
link: http://vninja.net/virtualization/installing-vsphere-management-assistant-vma/
slug: installing-vsphere-management-assistant-vma
title: Installing vSphere Management Assistant (vMA)
wordpress_id: 868
categories:
- Howto
- Virtualization
tags:
- ESX
- ESXi
- Ops
- vCLI
- Virtual Appliance
- vMA
- VMware
- vSphere
---

The _vMA_ is a Virtual Appliance that you can download from [VMware](http://communities.vmware.com/community/vmtn/vsphere/automationtools/vima). It's primary function is to enable command line based management of your ESX/ESXi systems.

Basically this is a pre-packaged virtual machine that  includes _vCLI_ and the _vSphere SDK for Perl_, which means that you don't have to build your own management VM or install these tools locally on a management station.

**vMA is in many regards seen as a replacement for the ESX Service Console which no longer is  present in ESXi.**


## Downloading vSphere Management Assistant (vMA)


Basically there are two methods you can use when deploying _vMA_.



	
  1. Download the .OVF file to your local machine and upload it to your ESX/ESXi host.
This is useful if you have already downloaded the vMA appliance and plan on deploying it to several hosts/clusters. This saves you from downloading it several times, and you can even use PowerCLI to automate the deployment of vMA, but I'm not going into that scenario this time around.

	
  2. Deploy the .OVF file directly from vmware.com, via the vSphere Client, to your host.


In this post, I'll focus on how to deploy vMA using method 2 - Direct Deployment


## Direct Deployment of vMA


Start your vSphere Client and connect to your host or vCenter.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-01-300x194.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-01.png)

Navigate to "_File_", find and select the _"Deploy OVF template_" option

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-02-245x300.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-02.png)

This starts the OVF deployment wizard. In the "_Deploy from a file or URL_"  text box, enter the following URL: [http://download3.vmware.com/software/vma/vMA-4.1.0.0-268837.ovf](http://download3.vmware.com/software/vma/vMA-4.1.0.0-268837.ovf) and click on "_Next_".

**Note:** This URL is valid at the point of writing, but might change at a later date when a new version is released by VMware.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-03-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-03.png)

The URL is now validated, and you are presented with the OVF Template Details window, where you can review the settings defined in the OVF file.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-04-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-04.png)

Click "_Next_"  to view and accept the VMware EULA. After reading it throroughly click on "_Accept_" and then on "_Next_" again to continue

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-05-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-05.png)

Next up is the "**Name and Location**" screen, where you can customize the name of your vMA instance. If you are deploying the several vMA instances to the same host/vCenter, you will need to change this to prevent naming conflicts.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-06-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-06.png)

After naming your vMA, click on "_Next_" again, and you'll get presented with the "**Disk Format**" screen. Here you can select between thin provisioned or thick provisioned disks, for this installation I chose to change it to thin provisioned, but the default is thick provisioned disks.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-07-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-07.png)

After making your selection, we once again click on "_Next_" and now we're nearly there! Review the summary screen to make sure you have selected the right options and  click on "_Finish_" to finally  start the download and deployment of your vMA.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-08-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-08.png)

The download starts, and a nice little progress window shows you how far along you are.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-09-300x134.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-09.png)

Of course, the time it takes to deploy vMA this way is highly dependant on available bandwidth and download speeds. In this particular environment the download is estimated to take approximately 39 minutes.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-10-300x93.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-10.png)

**Tip:** If you want to deploy vMA in this manner, to several hosts, you can place the downloadable vMA .OVF file on an accessible file share or http server and serve your local path or URL to your host via the vCenter Client as well. This is particularly useful in scenarios where your vCenter Client doesn't have internet access or if you want to speed up deployment by downloading it only once, but without scripting it.

Once the vMA has finished downloading and installing, it will pop up inside your vSphere Client on the host you deployed it to.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-11-300x194.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/Deploy-vMA-11.png)



### vMA Initial Configuration


The first time you start vMA, it fires a configuration wizard to help you configure it.
The wizard guides you through the network setup and setting the vi-admin user password.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/configure-vMA-01-300x166.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/configure-vMA-01.png) 
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/configure-vMA-02-300x166.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/configure-vMA-02.png)

And there it is. Now you can use your favorite SSH client ([Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/)) to connect to the vMA, or by using the console in the vSphere Client.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/configure-vMA-03-300x179.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/configure-vMA-03.png)

For details on using vMA and vCLI see the [vSphere Management Assistant (vMA) site](http://communities.vmware.com/community/vmtn/vsphere/automationtools/vima). You can even add ESX/ESXi and vMA to your Active Directory and use that as an authentication source, but I'll leave that for another post on another day.
