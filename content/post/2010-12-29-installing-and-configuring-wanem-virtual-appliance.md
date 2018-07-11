---
author: Christian Mohn
comments: true
date: 2010-12-29 14:21:36+00:00
layout: post
slug: installing-and-configuring-wanem-virtual-appliance
title: Installing and Configuring WANem Virtual Appliance
url: /virtualization/installing-and-configuring-wanem-virtual-appliance/
wordpress_id: 230
categories:
- Howto
- Network
- Virtualization
tags:
- Free Tools
- HowTo
- Networking
- Ops
- Simulation
- vCenter
- Virtual Appliance
- VMware
---





In a previous post, Using the [WANem WAN Emulator Virtual Appliance](http://vninja.net/network/using-the-wanem-wan-emulator-virtual-appliance/), I've talked about how I've successfully used WANem to emulate different WAN scenarios.

Since I work for a shipping company, the ability to emulate [VSAT](http://en.wikipedia.org/wiki/Very_small_aperture_terminal) conditions are especially useful for testing and proof-of-concept scenarios.

You can use WANem a couple of different ways but my setup is pretty simple but does the job perfectly.


#### Downloading WANem


I have chose to use the [WANem Virtual Appliance](http://wanem.sourceforge.net/download-vma.html) running in a virtual machine hosted by VMware Workstation. If you don't have Workstation handy, VMware Player will also do the job. You don't need to use the virtual appliance, as WANem also offers a bootable ISO download that you could boot your VM from


#### Starting and configuring WANem


First off, extract the _WANemv2.1.zip_ file, and place the virtual machine files in a know location. My choice is _c:\virtual machines\_ but you will have to adjust that for your specific environment.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-1-300x131.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-1.png)

Secondly, fire up VMware Workstation and chose the "Open existing VM or Team" option

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-2-300x210.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-2.png)

In the file dialog box browse to where you extracted the virtual machine files and select "Open"

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-3-300x210.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-3.png)

I like to run the WANem VM in bridge mode, since this gives the VM it's own IP address from my DHCP server. This comes in handy later when we setup what traffic we want to route through WANem.
By default the VM is configured in "Bridge Mode" which is the network mode we want, so go ahead and start the VM by pressing the _"Power on this virtual machine"_ option.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-4-300x137.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-4.png)

The WANem VM starts to boot up, but it stops asking if you want to configure all network interfaces via DHCP.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-5-300x198.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-5.png)

As I use DHCP in my network, I don't have to specify any network information so I hit _'y'_ to let it continue booting.
Once again the boot process stops, this time asking you to specify a root password for the VM. Provide your own password twice, and the boot process is finished. Don't worry about the password strength, or even if you remember it later. The VM boots on a pre-configured Knoppix LiveCD, and the next time you start it it will ask for a new password.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-6-300x180.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-6.png)

If you don't see the IP address that WANem has received from your DHCP server, type _status_ and hit enter, and it will show you the IP it has been assigned.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-7-300x201.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-7.png)

Open your favorite browser, and point it to _http://WANemIP/WANem_ (in my case this was http://192.168.5.90/WANem)

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-8-300x220.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-8.png)

And there it is, WANem is up and running. We'll leave it as is for the time being, and start routing traffic through it as is.


#### Running IP traffic trough WANem


You have a couple of choices when it comes to routing your traffic through WANem. The easiest setup si where you set WANem as your default gateway and route _all_ traffic through it. Remember though that this only comes into play if your destination is on another subnet than the one you are currently connected to. In many scenarios this isn't desirable, and I prefer to route specific traffic through WANem and not have to play around with my DHCP assigned settings.

What I usually do, is to drop into _cmd_ in administrator mode, and issue the following command:

**_route add {destination IP} mask 255.255.255.255 {WANem IP}_**

This routes all traffic from the computer I run the command on, through the WANem IP address, and from there on it goes to it's original destination. This allows it to work on local subnet traffic, as well as remote traffic, and it will not affect any other IP traffic from that computer to the rest of the network.

Obviously, we need a destination as well. For this lab setup I decided to route the traffic to my local printer through WANem. It's ip is 192.168.5.12, so the command to run inside cmd as administrator is:

**_route add 192.168.5.12 mask 255.255.255.255 192.168.5.90_**

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-9-300x151.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-9.png)

Now, all traffic from my local computer to my printer is routed through WANem. For now, nothing much happens. WANem receives the data on it's network card, and promptly sends it on it's merry way to the final destination; my printer. No innocent packets have been mauled or otherwise hurt in the process, at least not so far.



If you want to route _everything_ even local subnet traffic through WANem, enter the following command:

**_route add 0.0.0.0 mask 0.0.0.0 {WANem IP}_**
See [this commen](http://vninja.net/virtualization/installing-and-configuring-wanem-virtual-appliance/#comment-1556)t for context**_.
_**


#### Tweaking WANem settings


Finally, the fun part! Now we can start playing around with those innocent little network packets that flow through the network. Ever wanted to know what happens if you try to print to a printer over a 128kb WAN link? Well, now you can! I would strongly advise against trying to print your über 120 slide PowerPoint presentation at this point, but you can if you want to. Possibly.

I'm not going to try to print over the link as it is now, we'll have to settle for a little demonstration using much simpler means.

Tool of choice the ever faithfull _ping_. Basically I'll demonstrate how WANem fiddles with your network traffic. First off, a just a normal ping of my printer with no edits done by WANem what so ever:

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-10-300x151.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-10.png)

As you can see, as this is on my local network, the response time is great at 1ms and no dropped packets.

Now we can open the WANem web interface, and start messing about. Click on "Advanced Mode" and then on the "Start" button to bring up the web interface.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-12-300x220.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-12.png)

The next screen brings up a lot of options, and we're going to tweak a couple of them.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-12a-300x220.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-12a.png)

I set the _"Delay Time (ms)"_ to 200ms and the _"Loss %"_ to 25%, and hit _"Apply Settings"_. When we turn back to our cmd window, we should see instant results in our ping monitor.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-12b-300x151.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-12b.png)

As you can see, the response time is now approximately 200ms and the packet loss is about 25%, in accordance with the settings I've applied. Of course, as you can see, there are lots of other settings to play around with as well.

WANem also allows you to save your settings, so if you find your own perfect little real world emulation scenario you can export the settings and re-apply them later if you wish.


#### Cleaning house


When testing is over, be sure to remove your static route(s) again. If you don't, and you power down your WANem appliance you simply will not be able to reach your destination(s) until it's powered on again. Thankfully, removing the static routes are as easy as adding them in the first place. The command to run, in an administrative mode cmd is:

**_route delete {destination IP}_**

[![](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-13-300x151.png)](http://vninja.net/wordpress/wp-content/uploads/2010/12/WANem-13.png)


#### Conclusion


[WANem](http://wanem.sourceforge.net/) is a very nice, and simple to set up, little appliance that could make it much easier planning deployments and emulating real world scenarios. After all, do you know how your applications behave if they get high latency connections? Or if you get [jitter](http://en.wikipedia.org/wiki/Jitter#Packet_jitter_in_computer_networks) problems or even dropped packets?

Give it a test, you might just get a bit surprised at the results you get.

