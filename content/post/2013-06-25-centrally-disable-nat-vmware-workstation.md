---
author: Christian Mohn
comments: true
date: 2013-06-25 08:52:03+00:00
layout: post
slug: centrally-disable-nat-vmware-workstation
title: Centrally Disable NAT in VMware Workstation
url: /vmware-2/centrally-disable-nat-vmware-workstation/
wordpress_id: 2601
categories:
- VMware
tags:
- GPMC
- Group Policy
- Group Policy Preferences
- management
- VMware
- VMware Workstation
- Windows
- Workstation
---

A fellow IT-professional, who works with the non-wired flavor of networking, contacted me with the following scenario:

A group of users, developers in this case, have VMware Workstation installed on their laptops. This makes it easy for them to manage, test and develop their applications in a closed environment without having to install a bunch of tools/services on their centrally managed laptop environment. An excellent use case for VMware Workstation if there ever was one.

So far, so good. The problem in this particular case was that due to security policies in the network infrastructure there was a need to disable the NAT networking possibilities in VMware Workstation.


<blockquote>Network address translation (NAT) configures your virtual machine to share the IP and MAC addresses of the host. The virtual machine and the host share a single network identity that is not visible outside the network. NAT can be useful when you are allowed a single IP address or MAC address by your network administrator. You might also use NAT to configure separate virtual machines for handling http and ftp requests, with both virtual machines running off the same IP address or domain. See Network Address Translation (NAT).</blockquote>


[![VMware Workstation NAT Configuration ](/img/GUID-4C1FE8E1-9C52-4A43-9C36-97AEC38C737B-high-1-300x125.png)](http://pubs.vmware.com/workstation-9/index.jsp#com.vmware.ws.using.doc/GUID-89311E3D-CCA9-4ECC-AF5C-C52BE6A89A95.html) VMware Workstation NAT Configuration[/caption]

Since the VM shares the host MAC address and IP, blocking network access from the VM is not trivial in this scenario.

Thankfully, in VMware Workstation for Windows, NAT is provided through a Windows Service that we can manipulate. By disabling the "_VMware NAT Service_" we can ensure that NAT does not work, and that the only real alternative is to run the VM in "Bridged Mode".

Bridged Mode makes it easier for network admins to manipulate access, since the virtual network adapter is exposed to the switches with their own MAC address, and thus possibly also their own IP address, and the VM is not "hidden" behind the hosts MAC. For instance, this makes it possible for the network gurus to limit the VMs physical network access to internet access only, and not exposing the internal network to the VM.

Running around disabling the "_VMware NAT Service_" on all clients that run VMware Workstation is no fun job, so naturally we need to find a way to automate this as well.

**Enter Group Policy Preferences!**



	
  1. On a computer that has VMware Workstation installed, run the Group Policy Management Console and create a new GPO.[![Group Policy Management Console](/img/1-300x277.png)](/img/1.png)

	
  2. In Computer Configuration > Preferences > Control Panel Settings select Services

	
  3. In the menu click on Action > New > Service and and click on  “…” next to the Service Name field

	
  4. Select the "VMware NAT Service"and click “Select”[![Services](/img/5-300x191.png)](/img/5.png)

	
  5. Set the Startup mode to "disabled"

	
  6. Assign this new Group Policy Preference to the OU that the clients that have VMware Workstation installed on resides in, and the next time the policies are refreshed, the "VMware NAT Service" should be set to disabled.Note: This might require a reboot of the client.

	
  7. Profit.


And there it is, a workaround on how to disable the possibility for VMs running in VMware Workstation utilizing NAT mode. _A bit of a hack, but it works._


## Wishlist


_I really wish VMware would include the possibility to configure and manage multiple VMware Workstation for Windows installs, through Group Policy and Group Policy Preferences._

The ability to centrally manage configurations and settings would be a welcome addition to this already excellent piece of software, and I am sure that I am not alone in asking for this possibility. So how about it VMware, yay or nay?
