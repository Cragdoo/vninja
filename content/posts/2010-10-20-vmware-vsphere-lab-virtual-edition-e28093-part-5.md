---
author: cmohn
comments: true
date: 2010-10-20 13:31:51+00:00
layout: post
link: http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%e2%80%93-part-5/
slug: vmware-vsphere-lab-virtual-edition-%e2%80%93-part-5
title: 'VMware vSphere Lab: Virtual Edition – Part 5'
wordpress_id: 538
categories:
- Howto
- Virtualization
tags:
- ESX
- labs
- vCenter
- VMware
- VMware Workstation
- vSphere
---

![](/images/logos/vmware-logo.gif)This is the fifth post in a series outlining how to set up your own little virtualized **Virtual vSphere Lab**, if you missed [part one](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-1/), [part two](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-part-2/), [part three](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-3/), or [part four](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-4/),  be sure to check them out first!

Ideally we would now install Microsoft Active Directory Domain Services (AD DS) on the Windows Server 2008 R2 VM we created in [part four](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-4/), but since VMware vCenter Server 4.1 also installs AD LDS (previously known as Active Directory Application Mode (ADAM)) it can not be installed on a AD Domain Controller. 

This means that we either need to install a second VM with Windows Server 2008 R2 on, and install and configure Active Directory Domain Services (AD DS) on it, or run without an available Active Directory domain. Running a second Windows Server 2008 R2 install will consume a lot of resources, especially memory, which might not be available to you in your lab environment. For now, I've decided to go on without it.
<!-- more -->
Some advanced features, that require Active Directory, will therefore not be available to us in the current lab setup.

These features include



	
  * Active Directory Integration

	
  * Linked Mode Virtual Center Installs

	
  * Guided Consolidation


There are also a couple of other things that we need to consider when deciding to not to set up a domain for the lab. The most important thing is that this means that we won't automatically have any name resolution services available. Since features like High Availability (HA) is very dependent on DNS, this is the route I decided to go. Another, but smaller, caveat is that we will have to use locally created users when connecting to vCenter, but that is not a big deal in such a small environment.

In addition to DNS, it's a good idea to install DHCP as well, as the DHCP server in Windows Server 2008 R2 can register your DHCP clients in DNS for you, and thus eliminate the need to provide static IPs to your ESXi hosts or VMs.

Lets get down to business, the first order of the day;


#### Isolating the server network and assigning static IP


Installing DHCP is pretty straight forward, you add the role and configure it, and you're done. In this case, however, there are a few other things that need to be done before we setup a DHCP server and let it run wild in your environment. Setting up a "rouge" DHCP server has the potential to wreck havoc in your network, so you need to make sure there is no spillage of IP address configuration to your live network. To make sure this doesn't happen, we need to change some networking settings for your VMs and isolate them in their own environment, before we install anything else.

To do this open VMware Workstation, and find your _lab-esxi1_ VM.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-1-300x179.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-1.png)

Select "**Edit virtual machine settings**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-2-300x179.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-2.png)

In the "**Virtual Machine Settings**" window, find "**Network Adapter**" and change the value from "NAT" to "**Host-only**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-3-300x259.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-3.png)

Click "**OK**". Make sure that "**Network Adapter**" is set to "**Host-only**".

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-4-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-4.png)
Repeat this step for _lab-esxi2_, and _lab-vc1_ as well.

Now, your VMs are isolated from your live network, but can still interact with each other inside their own little bubble of fuzzy IP-goodness.


#### Assigning static IP


Power up the _lab-vc1_ Windows Server VM, and log on. Before we install DHCP, we need to assign a static IP. In order to be good IP citizens, we should chose from one of the available private pools of IP adresses for our lab setup. [RFC1918](http://en.wikipedia.org/w/index.php?title=Private_network#Private_IPv4_address_spaces) defines a couple of IP blocks we can use, and in this case I decided to go with the **192.168.1.x** range, and use the following setup:
<table >
  <tbody >
    
    <tr >
      Hostname
      IP
      Subnet mask
      Gateway
    </tr>
    <tr >
      
<td >lab-vc1
</td>
      
<td >192.168.1.10
</td>
      
<td >255.255.255.0
</td>
      
<td >192.168.1.10
</td>
    </tr>
  </tbody>
</table>
To assign this static IP adress to the server, click on "**Configure networking**".

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-5-300x85.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-5.png)

This opens the "**Network Connections**" window, where you will see your "**Local Area Connection**" interface.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-6-300x120.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-6.png)

Right-click the  "**Local Area Connection**" interface and select "**Properties**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-6-300x120.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-6.png)
Highlight "**Internet Protocol Version 4 (TCP/IPv4)**", and select "**Properties**".

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-8-270x300.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-8.png)

Select "User the following IP address:" and fill out the information from the table above (We do not need a default gateway in this isolated network, so leave that blank).
[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-9-270x300.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-9.png)

Since we are installing DNS on the same server in a little while, we might as well put the server IP (192.168.1.10) in both preferred and alternate DNS server boxes right now as well.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-10-270x300.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Network-10.png)

Click "**OK**" and then "**Close**".
We have now assigned a static IP to the server, and can continue with installing DHCP.



#### Installing DHCP


Finally, we can get on with installing the DHCP service for our lab. To do this, start Server Manager which is available on the right hand side of your "**Start**" button.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Server-Manager-1.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Server-Manager-1.png)

Click on "**Roles**" and then on "**Add Roles**" . This starts the "**Add Roles Wizard**". Click on "**Next**", and the available server roles are presented.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Add-Roles-2-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Add-Roles-2.png)

Find **DHCP Server**, and select it.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Add-Roles-2-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-Add-Roles-2.png)

Click "**Next**" and you'll get presented with a nice little screen that explains DHCP. We'll skip that pretty quickly, and click "**Next**" again. The next thing in line, is a screen where you select which network adapter the DHCP service should bind to.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-1-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-1.png)

Since we only have one network interface in this particular server, we'll just accept the defaults and click "**Next**" again. The "**Specify IPv4 DNS Server Server Settings**" appear, and we need to fill out the "**Parent Domain**" name. Since this is our Virtual vSphere Lab, I named it _vLab_.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-2a-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-2a.png)

Fill out the required information and click on "**Next**". We don't need WINS based name resolution in our lab setup, so we'll leave the default of "**WINS is not required for applications on this network**" as is, and yet again click "**Next**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-3-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-3.png)

Now, we need to configure our DHCP scope, to let the DHCP server know which IP addresses it is responsible for and provides the DHCP client with.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-4-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-4.png)

Click on the "**Add**" button

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-5-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-5.png)

This is where we actually get to do something, instead of just clicking along the whole time. Fill out the following information:
<table >
<tbody >
<tr >
DHCP Configuration

</tr>
<tr >

<td >Scope name
</td>

<td >vLabScope
</td>
</tr>
<tr >

<td >Starting IP address
</td>

<td >192.168.1.50
</td>
</tr>
<tr >

<td >Ending IP adress
</td>

<td >192.168.1.254
</td>
</tr>
<tr >

<td >Subnet Type
</td>

<td >Wired (Lease duration will be 8 days)
</td>
</tr>
<tr >

<td >Subnet mask
</td>

<td >255.255.255.0
</td>
</tr>
<tr >

<td >Default gateway (optional)
</td>

<td >192.168.1.10
</td>
</tr>
<tr ></tr>
</tbody>
</table>
It should look like this:

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-6a-300x256.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-6a.png)

Click "**OK**" and verify that everything looks like it should

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-7-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-7.png)

Click "**Next**" and select "**Disable DHCPv6 stateless mode for this server**", there is no need for IPv6 DHCP in our lab at the moment.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-8-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-8.png)

and click "**Next**" again.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-9a-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-9a.png)

Review the settings you have configured, and click on "**Install**" to actually get the install going.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-10-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-10.png)
[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-11-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DHCP-11.png)

Click "**Close**" and thats it, DHCP has now been installed and configured! Well done!



#### Installing DNS


If Server Manager is still running, click on "**Add Roles**". If not, start Server Manager first.

Click on "**Roles**" and then on "**Add Roles**" . This starts the "**Add Roles Wizard**". Click on "**Next**",

Click "**Next**" and in the "**Select Server Roles**" windows, select "**DNS Server**"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DNS-1a-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DNS-1a.png)

Review the "Introduction to DNS Server" screen, before continuing by clicking on "**Next**".

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DNS-2a-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DNS-2a.png)

Click on "**Install**" and the install continues.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DNS-3-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DNS-3.png)
[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DNS-4-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DNS-4.png)

Click "**Close**" to end the installation procedure for DNS Server.

Next up, configuring DNS. Navigate to **Start -> Administrative Tools -> DNS**

[![](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DNS-5-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/10/Virtual-vSphere-Lab-Installing-W2K8-DNS-5.png)

And that should be it! We now have working DHCP and DNS servers in our little lab network, ready to provide your ESXi hosts with the information they need!

In [part six](http://vninja.net/virtualization/vmware-vsphere-lab-virtual-edition-%E2%80%93-part-6/), I'll be going through installing VMware vCenter Server 4.1. Stay tuned!


Now that
