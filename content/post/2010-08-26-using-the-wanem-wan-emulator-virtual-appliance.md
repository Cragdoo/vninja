---
author: Christian Mohn
comments: true
date: 2010-08-26 11:38:00+00:00
layout: post
slug: using-the-wanem-wan-emulator-virtual-appliance
title: Using the WANem WAN Emulator Virtual Appliance
url: /network/using-the-wanem-wan-emulator-virtual-appliance/
wordpress_id: 192
categories:
- Network
tags:
- Free Tools
- Networking
- Ops
- Simulation
- vCenter
- Virtual Appliance
- VMware
---

During preparation and preliminary information gathering for a new internal project, I had a need to emulate various networking conditions and scenarios. More specifically I'm looking at the possibility of running the vCenter Client over high latency satellite links, with varying bandwidth availability and even packet loss scenarios.

Obviously the best way of testing this, in a controlled environment, is to use some kind of WAN emulator that lets you control the various networking characteristics. [WANem](http://wanem.sourceforge.net/) is a free WAN emulator and it even comes as a VMware [virtual appliance](http://wanem.sourceforge.net/).

Setup is pretty straight forward, and I won't get into the detailed instructions at this point. If someone requests it, perhaps I'll make a _HOWTO_ post later on.

After the WANem Virtual Appliance has been started and setup in your network environment, all you have to do is to route your traffic through it. In my test environment, I decided to route all traffic between my local computer and my vCenter Server through the WANem appliance. Doing so is pretty straight forward; Open up a cmd window, with administrator privileges, on your local computer and use the route command to force traffic through WANem:

[![](/img/WANem-AddRoute-300x125.png)](/img/WANem-AddRoute.png rel=)

the command itself is:
**_route add {destination IP} mask 255.255.255.255 {WANem IP}_**

To tune the network properties of the traffic going through WANem, open the WANem admin page in your browser and work some magic. The screenshots below are from the **advanced tab**:

[![WANem Advanced Mode Screenshot #1](/img/WANem-Advanced1-300x220.png)](/img/WANem-Advanced1.png)[![WANem Advanced Mode Screenshot #2](/img/WANem-Advanced2-300x220.png)](/img/WANem-Advanced2.png)

As a simple test, I decided to add **500ms latency** (delay time) and a **packet loss of 25%**, and as you can see from the video below it works as expected

                
(Video has been scaled to fit, watch it in fullscreen mode for details).



#### Conclusion



If you need to test out how your applications or networking infrastructure works when issues like latency, jitter and even dropped packets affects your clients, WANem seems like an easy and free route (pun intended) for testing purposes.
