---
author: Christian Mohn
comments: true
date: 2013-04-25 11:10:33+00:00
layout: post
slug: adding-secondary-nic-vcenter-5-1-appliance-vcsa
title: Adding a secondary NIC to the vCenter 5.1 Appliance (VCSA)
url: /virtualization/adding-secondary-nic-vcenter-5-1-appliance-vcsa/
wordpress_id: 2549
categories:
- Virtualization
tags:
- ESXi
- Hack
- Networking
- vCenter
- VCSA
- Virtualization
- vSphere
---

While building my lab environment, I ran into a situation where I wanted to have a completely sealed off networking segment that had no outside access.

This is a trivial task on it`s own, just create a vSwitch with no physical NICs attached to it, and then connect the VMs to it. The VMs will then have interconnectivity, but no outside network access at all.

In this particular case, I was setting up a couple of nested ESXi servers that I wanted to connect to the "outside" vCenter Appliance (VCSA). This VCSA instance was not connected to the internal-only vSwitch, but rather to the existing vSwitch that as local network access.

Naturally, the solution would be to add a secondary NIC to the VCSA, and connect that to the internal-only vSwitch.

**It turns out that adding a secondary NIC to a VCSA instance, isn`t as straight-forward as you might think.** Sure, adding a new NIC is no problem through either the vSphere Client, or the vSphere Web Client, but getting the NIC configured inside of VCSA is another matter.

If you add a secondary NIC, it will turn up in the VCSA management web page, but you will not be able to save the configuration since the required configuration files for _eth1_ is missing.

In order to rectify this, I performed the following steps:




    
  1. Connect to the VCSA via SSH (default username and password is root/vmware)

    
  2. Copy _/etc/sysconfig/networking/devices/ifcfg-eth0 to /etc/sysconfig/networking/devices/ifcfg-eth1_

    
  3. Edit _ifcfg-eth1 _and replace the networking information with your values, here is how mine looks:
[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
DEVICE=eth1
BOOTPROTO='static'
STARTMODE='auto'
TYPE=Ethernet
USERCONTROL='no'
IPADDR='172.16.1.52'
NETMASK='255.255.255.0'
BROADCAST='172.16.1.255'
[/cc]

    
  4. Create a symlink for this file in /etc/sysconfig/network[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
ln -s /etc/sysconfig/networking/devices/ifcfg-eth1 /etc/sysconfig/network/ifcfg-eth1[/cc]

    
  5. Restart the networking service to activate the new setup:[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
service network restart[/cc]Check the VCSA web management interface to verify that the new settings are active



![Client 2013-04-25 10-54-37](http://vninja.net/wordpress/wp-content/uploads/2013/04/Client-2013-04-25-10-54-37-300x200.png)

By adding a secondary NIC, configuring it and connecting it to the isolated vSwitch I was now able to add my sequestered nested ESXi hosts to my existing VCSA installation.



[![Client 2013-04-25 13-07-01](http://vninja.net/wordpress/wp-content/uploads/2013/04/Client-2013-04-25-13-07-01-300x116.png)](http://vninja.net/wordpress/wp-content/uploads/2013/04/Client-2013-04-25-13-07-01.png)

There may be several reasons for a setup like this, perhaps you want your VCSA to be available on a management VLAN but reach ESXi hosts on another VLAN without having routing in place between the segmented networks, or you just want to play around with it like I am in this lab environment.

Disclaimer:

_Is this supported by VMware? Probably not, but I simply don`t know. Caveat emptor, and all that jazz._



#### Update February 2016:



This post is written with VCSA5.x in mind, and is not tested on VCSA 6.x. William Lam has posted [Caveats when multi-homing the vCenter Server Appliance 6.x w/multiple vNICs](http://www.virtuallyghetto.com/2016/02/caveats-when-multi-homing-the-vcenter-server-appliance-6-x-wmultiple-vnics.html) with information on what caveats exist if you are looking to do this with the newer v6.x infrastructure.
