---
author: cmohn
comments: true
date: 2011-03-21 19:50:02+00:00
layout: post
link: http://vninja.net/virtualization/installing-and-configuring-vmware-vcenter-operations/
slug: installing-and-configuring-vmware-vcenter-operations
title: Installing and configuring VMware vCenter Operations
wordpress_id: 945
categories:
- Howto
- Virtualization
tags:
- Maintenance
- Monitoring
- Ops
- realworld
- vCenter
- vCenter Operations
- Virtualization
- vSphere
---

VMware vCenter Operations was released to the general public a week or so ago and is [available for download](http://www.vmware.com/products/vcenter-operations/overview.html) right now. As usual you can download a 60 day trial, and get started immediately.

Like other recent management utilities from VMware, vCenter Operations comes in the form of a .OVF template (like [vCMA](http://communities.vmware.com/community/beta/vcmobileaccess)/[vMA](http://www.vmware.com/support/developer/vima/)).


### Installing VMware vCenter Operations


Download [VMware vCenter Operations](http://www.vmware.com/products/vcenter-operations/overview.html) and import by starting vCenter Client, navigate to the “**File**” menu and select “**Deploy OVF template…**”

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/1.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/1.png)

Browse to the download location, and find the “_VMware-vcops-1.0.0.0-373027_OVF10.ova_” file. Select it and click open.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/2-300x208.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/2.png)

Click on “**Next**” and review the details.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/3-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/3.png)

Hit “**Next**” once more, and click on “**Accept**” to accept the VMware EULA and enable the “**Next**” button.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/4-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/4.png)

Specify the name and location of the VMware vCenter Operations VM, and click “**Next**” to continue.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/5-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/5.png)
Now we get to select which host or cluster the VM should be deployed to. Make your choices, and click on “**Next**”
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/6-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/6.png)
Select your preferred resource pool, if you have any, and once again click “**Next**”
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/7-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/7.png)
Now select your preferred datastore, and guess what? We get to click “**Next**” one more time!
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/8-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/8.png)
Decide if you want a thin or thick provisioned VM, the default is thick but I went with thin provisioned in this particular setup.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/9-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/9.png)
The last configuration item, for now, is to map the networks. Select your network mappings and click on “**Next**”.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/10-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/10.png)
Review the final setup screen, and once you are satisfied that your settings are correct, click on “**Finish**” to start the OVF template import.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/11-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/11.png)
The import starts, and after a few minutes it should be ready to go!
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/16-300x134.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/16.png)
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/19-300x93.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/19.png)
Success!
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/20-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/20.png)
Time to start it up!


### Configuring VMware vCenter Operations


After the vCenter Operations VM has finished booting, it displays a little information screen showing the IP address and other tidbits of information. The most important piece of information right now is the DHCP assigned IP address. _Make a note of that IP for later._
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/21-300x166.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/21.png)

To make sure we don’t run into problems with time synchronization we need to make sure that the vCenter Operations VM time is synchronized with the ESX host time. To do so, right click on the VMware vCenter Operations VM inside of the vCenter Client, select “**Edit Settings**”.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/21-2-275x300.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/21-2.png)

Select the “**Options**” tab, and find the VMware Tools section.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/21-3-300x264.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/21-3.png)
Find the “**Synchronize guest time with host**” option, and select it.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/21-4.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/21-4.png)
Open the vCenter Operations web page in a browser, and log in. The default username/password for vCenter Operations is _admin/admin_
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/22-300x227.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/22.png)
Log in, and follow the directions on screen to change the default username/password.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/23-300x227.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/23.png)
_The new password must be at least 8 characters, and at least one digit and one character._
**Note: This also changes the root account password for the vCenter Operations VM.**

Next up is configuring the vCenter Operations connection to the vCenter.

Fill out the vCenter Server information form, with information pertinent to your infrastructure.
_Note that the registration credentials needs to have administrator privileges on the vCenter Server. You can use the same credentials for both registration and collection, or you can differentiate them if required in your environment._
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/24-300x227.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/24.png)
Click on "**Save**", and a test is performed to make sure that the information provided is correct.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/25-300x218.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/25.png)
If registration is successful, a new popup appears explaining that you need to use the vSphere Client to assign the vCenter Operations licenses.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/26-300x227.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/26.png)
Click on “**Ok**” and the vCenter Operations setup dashboard appears in your browser.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/27-300x227.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/27.png)
Go back to your vCenter Client and navigate to the “**Home**” screen.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/28-300x288.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/28.png)
You should now see the new "**vCenter Operations**” icon under “**Solutions and Applications**”. If it does not appear immediately, close the vCenter Client and restart it to have it pick up the installation.
To install the vCenter Operations license, go to “**Home**” and find the “**Licensing**” icon.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/29-300x288.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/29.png)
Click on it, and change the “**View by:**” option to “**Asset**”
Right click on “**vCenter Operations**” and select “**Change License Key**”
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/30-300x44.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/30.png)
Select “**Assign new license key to this solution**”, click on “**Enter Key…**” and enter your license key and optionally a label for the key. Click on “**_O_K**” to return to the “**_Assign License_**” window, and click on “**OK**” again to install the license.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/31-300x111.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/31.png) [![](http://vninja.net/wordpress/wp-content/uploads/2011/03/32-300x289.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/32.png)
Your license should now be installed and active.

Go back to the vCenter Client “**Hom**e" screen and find the vCenter Operations icon under “**Solutions and Applications**”. Click on it, and vCenter Operations should already be active monitoring your infrastructure.
[![](http://vninja.net/wordpress/wp-content/uploads/2011/03/33-300x288.png)](http://vninja.net/wordpress/wp-content/uploads/2011/03/33.png)

Thats it! You now have VMware vCenter Operations running in your environment. For details on how it works and reports your operations refer to the [VMware vCenter Operations official documentation](http://www.vmware.com/products/vcenter-operations/overview.html). Eric Sloof has also posted a couple of great videos in his [VMware vCenter Operations - Troubleshooting Workflow](http://www.ntpro.nl/blog/archives/1712-Video-VMware-vCenter-Operations-Troubleshooting-Workflow.html) post that gives an in-depth overview of what VMware vCenter Operations is capable of doing.
