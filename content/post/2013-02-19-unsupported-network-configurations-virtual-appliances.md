---
author: Christian Mohn
comments: true
date: 2013-02-19 23:50:20+00:00
layout: post
slug: unsupported-network-configurations-virtual-appliances
title: Unsupported Network Configurations in Virtual Appliances
url: /vmware-2/unsupported-network-configurations-virtual-appliances/
wordpress_id: 2378
categories:
- VMware
tags:
- fun
- labs
- Linux
- SUSE
- Unsupported
- Virtual Appliance
- VMware
- vSphere
---

My recent experience with setting up v[Center Operations Manager on a standalone ESXi host](http://vninja.net/vmware-2/vcenter-operations-manager-vcenter-deployment-dependency/), and the always excellent [William Lam](http://twitter.com/lamw)´s post [Automating VCSA Network Configurations For Greenfield Deployments](http://www.virtuallyghetto.com/2013/02/automating-vcsa-network-configurations.html) got me thinking.

There are several other appliances out there that require deployment to a vCenter, to be able to configure the networking options and not just default to DHCP. In many, and perhaps even most, cases you can work around that by running the _vami_set_network _command to change from DHCP to STATIC network configurations.

All of that is fine and dandy, and pretty straight forward, but there is one smallish caveat;

**You need root access to be able to reconfigure the networking**.

Without it, you will see error messages such as these (Shortened for abbreviation):

[cc lang="bash" width="95%" theme="blackboard" nowrap="0"]@localhost:~/opt/vmware/share/vami/vami_set_network eth0 STATICV4 192.168.5.98 255.255.255.0 192.168.5.1
/sbin/ifdown: line 233: /dev/.sysconfig/network/config-eth0: Permission denied
IOError: [Errno 13] Permission denied: '/opt/vmware/etc/vami/vami_ovf_info.xml'
Unable to set the network parameters
[/cc]

So, what if you don´t know the appliance root password?

Most virtual appliances are Linux based, and in this particular case the flavor used was SUSE Enterprise Linux 11.

To reset the root password on a grub based Linux appliance, like SUSE, follow the recipe below:

**Note:** As [William Lam pointed out](https://twitter.com/lamw/status/304016471829397504) this procedure only works if there no grub password set, if that´s the case download a LiveCD, mount the filesystem and run the password change from there. If the filesystem in the appliance is encrypted, well, then all bets are off.



	
  1. In the grub menu select the kernel you want to boot and press tab to shift focus to "Boot Options"

	
  2. Now type _**init =/bin/bash **_and press _Enter_ to continue.[![SUSE-PW-RESET-01](http://vninja.net/wordpress/wp-content/uploads/2013/02/SUSE-PW-RESET-01-300x202.png)](http://vninja.net/wordpress/wp-content/uploads/2013/02/SUSE-PW-RESET-01.png)

	
  3. You will see a prompt that looks like  **(none):/ #** in the terminal.

	
  4. Run the _passwd_ command in terminal to change the password for root.
[cc lang="bash" width="95%" theme="blackboard" nowrap="0"]
(none):/ # passwd
New Password:
Reenter New Password:
Password changed.
[/cc]


Reboot the appliance, let it boot up normally, and you should now be able to log on as **root**, with your newly configured password, and run the _vami_set_network _command to configure static IP adressing.

[cc lang="bash" width="95%" theme="blackboard" nowrap="0"]
localhost:~ # /opt/vmware/share/vami/vami_set_network eth0 STATICV4 192.168.5.19 255.255.255.0 192.168.5.1
eth0 device: Intel Corporation 82545EM Gigabit Ethernet Controller (Copper) (rev 01)
eth0 device: Intel Corporation 82545EM Gigabit Ethernet Controller (Copper) (rev 01)
Network parameters successfully changed to requested values
localhost:~ #
[/cc]

Do yet another reboot, and you should be up and running with a static IP configuration on an appliance deployed without the advanced OVF/OVA properties normally required for that kind of deployment.

**Note:** _This procedure is more than likely NOT supported by your vendor, and changing the root password might have other consequences for the appliance. If the vendor does not supply the root password i their documentation, there is likely to be a reason for that, but the procedure above shows that not supplying it does not actually prevent anyone from changing it. USE AT OWN RISK._
