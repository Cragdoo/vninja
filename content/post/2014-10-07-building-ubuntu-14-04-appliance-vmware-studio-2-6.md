---
author: cmohn
comments: true
date: 2014-10-07 23:26:03+00:00
layout: post
slug: building-ubuntu-14-04-appliance-vmware-studio-2-6
title: Building an Ubuntu 14.04 Appliance with VMware Studio 2.6
url: /vmware-2/building-ubuntu-14-04-appliance-vmware-studio-2-6/
wordpress_id: 3424
categories:
- VMware
tags:
- Appliance
- Hack
- Ubuntu
- Unsupported
- VMware Studio
---

VMware Studio 2.6 was released way back in March 2012,  and surprisingly there seems to be no new update in sight. While VMware Studio technically still works, even with newer versions of ESXi and vCenter, the supported operating systems for the appliances it can build is _somewhat_ outdated:


<!--more-->



  * Red Hat Enterprise Linux 5.5/6.0


  * SUSE Linux Enterprise Server 10.2/11.1


  * Ubuntu 8.04.4/10.04.1


  * Windows Server 2003 R2 / 2008 R2 w/SP1





### The Problem:



For a yet to be announced project we are working on internally at EVRY, we needed to build an appliance that was based on newer software packages and development tools. Recent events like heartbleed and shellshock also highlight the need to build new appliances on new and supported distributions.

Attempts at upgrading an existing Ubuntu 10.04.1 appliance to 14.04 failed miserably, due to architectural changes between the Ubuntu versions and how Virtual Appliance Management Infrastructure (VAMI) is installed by VMware Studio and in the end we were pretty much left to two options:





  1. Build the appliance from scratch, and lose VAMI, which was one of the primary reasons for building the appliance with VMware Studio in the first place.


  2. Find a way to build the appliance with Ubuntu 14.04, with VMware Studio.



Option 1 felt a bit like giving up, and option 2, well that was a challenge we couldn't just walk away from.



### The Solution:



Thanks to the brilliant mind of my coworker [Espen Ødegaard,](http://twitter.com/esodesod) we were able to come up with a set of solutions that does the trick.

First we added a new profile to VMware Studio connecting to the VMware Studio VM via SSH, and by issuing the following command:
{{< highlight bash >}}studiocli --newos --osdesc "Ubuntu14.04” --profile /opt/vmware/etc/build/templates/ubuntu/10/041/build_profile.xml
{{< /highlight >}}
Basically this creates a new OS template in VMware Studio by copying the existing Ubuntu 10.04.1 profile.

Next edit line 462 in _/opt/vmware/etc/build/templates/Ubuntu14.04/Ubuntu14.04.xsl_ and change _scd0_ to _sr0_.

{{< highlight bash >}}if ! mount -t iso9660 -r /dev/scd0 /target/${cdrom_dir} ; then \
if ! mount -t iso9660 -r /dev/hda /target/${cdrom_dir} ; then \
cdrom_mounted=0; \
fi ; \
{{< /highlight >}}

This fixes a problem with the appliance not being able to mount the Ubuntu installation iso when it is built. Place the Ubuntu 14.04 ISO in_ /opt/vmware/www/ISV/ISO_, and create a new Build profile using the VMware Studio Web Interface.

Now, before an appliance can be built, we need to fix some other problems that prevent VAMI from starting up, and preventing login to the VAMI web interface. First of, make sure that _libncurses5_ is added as package under **Application** -> **List of packages from OS install media**. Next, add the following to the _first boot script_ under **OS**->**Boot Customization**, we can move around those problems.

{{< highlight bash >}}
# Create symlinks required for Ubuntu 14.04 and VAMI

# Copy and symlink libncurses to the location VAMI looks for them

cp /lib/i386-linux-gnu/libncurses.so.5.9 /opt/vmware/lib/
rm /opt/vmware/lib/libncurses.so.5
ln -s /opt/vmware/lib/libncurses.so.5.9 /opt/vmware/lib/libncurses.so.5

# Symlink PAM libraries in order for them to work with VAMI
# This "unbreaks" authentication in the web interface
mkdir /lib/security
ln -s /lib/i386-linux-gnu/security/* /lib/security/
{{< /highlight >}}

For details on how all of this works, and how to create build profiles, check the official [VMware Studio documentation](https://www.vmware.com/support/developer/studio/studio26/studio_developer.pdf).

The build process should now complete successfully, and you should have an Ubuntu 14.04 based appliance built with VMware Studio!
![vStealth1](/img/vStealth1.png)
![vStealth2](/img/vStealth2.png)

_Note that all of this is completely unsupported by VMware, and you are pretty much on your own. Hopefully there will be a new version of VMware Studio available soon, and we won't have to rely on unsupported hacks to get it working with newer operating systems._
