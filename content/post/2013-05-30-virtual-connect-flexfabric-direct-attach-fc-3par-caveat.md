---
author: Christian Mohn
comments: true
date: 2013-05-30 21:08:45+00:00
layout: post
slug: virtual-connect-flexfabric-direct-attach-fc-3par-caveat
title: Virtual Connect FlexFabric and Direct-Attach FC 3Par Caveat
url: /storage-2/virtual-connect-flexfabric-direct-attach-fc-3par-caveat/
wordpress_id: 2588
categories:
- Storage
tags:
- 3Par
- BladeSystem
- Bug
- Fibre Channel
- HP
- SFP+
- storage
- StoreServ
- Virtual Connect
---

When configuring a new C7000 Blade Enclosure with a couple of FlexFabric 10Gb/24-port modules I ran into a rather annoying issue during setup.

HP Virtual Connect 3.70 introduced support for Direct-Attach setups of HP 3Par StoreServ 7000 storage systems, where you can eliminate the need for dedicated FC switches. For full details, have a look at [Implementing HP Virtual Connect Direct-Attach Fibre Channel with HP 3PAR StoreServ Systems](http://h20000.www2.hp.com/bc/docs/support/SupportManual/c03605173/c03605173.pdf).

This is excellent for setups where all your hosts are HP Blades, and you have a Virtual Connect FlexFabric setup. After all, less components means less complexity, right?

The problem I ran into is a bit strange though, and it took some time figuring out what was wrong. The HP 3Par StoreServ 7200 was racked, stacked and configured when the FC [SFP+](http://en.wikipedia.org/wiki/Small_form-factor_pluggable_transceiver)  modules where installed in the FlexFabric module, and I pretty much thought it would be plug and play from there to get the blades to talk FC to the HP 3Par after going through the Virtual Connect setup.

Sadly, that was not the case. It seems there is a bug in the web GUI for VC 3.70 that prevents getting a working setup. I know 3.75 is released, but nothing in the [release notes](http://bizsupport2.austin.hp.com/bc/docs/support/SupportManual/c03673099/c03673099.pdf) seem to indicate that this has been fixed in that release.

For some reason, the "Fabric Type" dropdown where you should be able to select either "FabricAttach" or "DirectAttach" is greyed out, thus preventing the proper configuration of the SAN Fabric in "DirectAttach" mode. It defaults to "FabricAttach", and in a Direct-Attach scenario that simply does not work. You will not be able to get a FC link and the SFP+ module will be listed as "unsupported".

[![SanFabric-1a](http://vninja.net/wordpress/wp-content/uploads/2013/05/SanFabric-1a-300x157.png)](http://vninja.net/wordpress/wp-content/uploads/2013/05/SanFabric-1a.png)





The solution was to create the SAN Fabric manually by using the Virtual Connect CLI interface. T
he following commands created the two fabrics required for redundancy (VC module in Bay 1 and in Bay 2)

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
add fabric Fabric-1-3PAR Bay=1 Ports=1 Type=DirectAttach
add fabric Fabric-2-3PAR Bay=2 Ports=1 Type=DirectAttach
[/cc]

As you can see, by using the _add fabric_ command it was possible to define the correct Fabric Type and I could then proceed to add Port 2 from Bay 2 to Fabric-1-3PAR and vice versa to create a fully redundant setup.

[![SanFabric-2](http://vninja.net/wordpress/wp-content/uploads/2013/05/SanFabric-2-300x76.png)](http://vninja.net/wordpress/wp-content/uploads/2013/05/SanFabric-2.png)

Why the VC GUI prevented me from setting the correct fabric type is beyond me, but for some reason it simply did not allow me to change this rather important setting, and prevented the setup from working without using the CLI for configuration.


