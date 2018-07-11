---
author: Christian Mohn
comments: true
date: 2011-09-15 09:18:44+00:00
layout: post
slug: network-simulation-in-vmware-workstation-8
title: Network Simulation in VMware Workstation 8
url: /virtualization/network-simulation-in-vmware-workstation-8/
wordpress_id: 1534
categories:
- Virtualization
- VMware
tags:
- Networking
- Ops
- VMware
- Workstation 8
---

In the new VMware Workstation 8 release, VMware has added a rudimentary network simulation setting where you can tweak bandwidth and packet loss for a given network card. Very useful when testing applications and servers and want to know how they react to network issues, or if you want to simulate a WAN link. I know this was available in Workstation 7 as well, but it used to be a team feature. Now it's per vNIC feature, which makes it much more useable.

Configuring it is very easy, but you need to know where to look to be able to find the feature.



### Configuring Network Adapter Advanced Settings in VMware Workstation 8






  * Find your VM, right click it and select settings
[![](http://vninja.net/wordpress/wp-content/uploads/2011/09/Network-Simulation-in-VMware-Workstation-8-1-150x150.png)](http://vninja.net/wordpress/wp-content/uploads/2011/09/Network-Simulation-in-VMware-Workstation-8-1.png)



  * Select the _Network Adapter_ and click on the "Advanced..." button
[![](http://vninja.net/wordpress/wp-content/uploads/2011/09/Network-Simulation-in-VMware-Workstation-8-2-150x150.png)](http://vninja.net/wordpress/wp-content/uploads/2011/09/Network-Simulation-in-VMware-Workstation-8-2.png)



  * This brings up the Network Adapter Advanced Settings window, where you can tweak the network settings including inbound/outbound bandwidth and packet loss percentage
[![](http://vninja.net/wordpress/wp-content/uploads/2011/09/Network-Simulation-in-VMware-Workstation-8-3-150x150.png)](http://vninja.net/wordpress/wp-content/uploads/2011/09/Network-Simulation-in-VMware-Workstation-8-3.png)
There are a number of predefined settings for bandwidth, making it easy to simulate various scenarios like ISDN, cable, leased T3 and so on. You can even modify the virtual network card MAC address in the same window, if you need to do that.


  * Tweak the settings, and the new bandwidth and packet loss settings will immediately be applied to the VM




### Configuring Network Adapter Advanced Settings in VMware Workstation 8: Video Demo





### Conclusion


I love this. In my day job I'm often faced with simulating how different applications work over some rather wonky WAN lines, and building this kind of feature set into VMware Workstation 8 makes a lot of sense. I do hope they improve it in the future though, as I really would like to see it add tweakable settings for latency as well, which often is the main killer in WAN environments. For now, I'll have to stick to [WANem for the latency simulation](http://vninja.net/virtualization/installing-and-configuring-wanem-virtual-appliance/), at least until VMware adds latency tweaking to VMware Workstation.
