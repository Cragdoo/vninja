---
author: Christian Mohn
comments: true
date: 2013-04-22 21:31:46+00:00
layout: post
slug: installing-dell-equallogic-multipathing-extension-module-mem-vsphere-5-1
title: Installing Dell Equallogic Multipathing Extension Module (MEM) in vSphere 5.1
url: /virtualization/installing-dell-equallogic-multipathing-extension-module-mem-vsphere-5-1/
wordpress_id: 2538
categories:
- Virtualization
tags:
- Dell
- Equallogic
- ESXi
- HowTo
- Multipath
- storage
- vSphere
---

Dell offers a Multipathing Extension Module (MEM) for vSphere, and in this post I´ll highlight how to "manually" install it on a ESXi 5.1 host. _I will not cover the network setup part of the equation, but rather go through the simple steps required to get the MEM installed on the hosts in question._

First of all, you need to download the MEM installation package. At the time of writing, the latest version is v1.1.2.292203, available from [equallogic.com](http://equallogic.com). One the archive file is aquired, unzip it and upload the _dell-eql-mem-esx5-1.1.2.292203.zip_ file to a shared location available in your environment. For the example below, I have used a VMFS datastore that is available to all the hosts in this particular cluster.

**Note: The host in question has already been put in maintenance mode, to make sure no VMs are actively using the storage paths while installing the module.**

The VIB file, which resides inside the _dell-eql-mem-esx5-1.1.2.292203.zip_, file can be installed using several techniques; By using the vMA, vSphere Command-Line Interface (vSphere CLI), vSphere Update Manager or even by logging in to the hosts via a SSH connection, and in this case I opted for doing it through SSH.

The command required to install the MEM is as follows:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
esxcli software vib install --depot /vmfs/volumes/vmfsvol/dell/dell-eql-mem-esx5-1.1.2.292203.zip[/cc]

A completed installation looks like this:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]

login as: root
Using keyboard-interactive authentication.
Password:
The time and date of this login have been sent to the system logs.

VMware offers supported, powerful system administration tools. Please
see www.vmware.com/go/sysadmintools for details.

The ESXi Shell can be disabled by an administrative user. See the
vSphere Security documentation for more information.
~ # esxcli software vib install --depot /vmfs/volumes/vmfsvol/dell/dell-eql-mem-esx5-1.1.2.292203.zip
Installation Result
Message: Operation finished successfully.
Reboot Required: false
VIBs Installed: Dell_bootbank_dell-eql-host-connection-mgr_1.1.1-268843, Dell_bootbank_dell-eql-hostprofile_1.1.0-212190, Dell_bootbank_dell-eql-routed-psp_1.1.1-262227
VIBs Removed:
VIBs Skipped:
~ #[/cc]

I then restart the hosts process, to make sure that the multipath module is activated.

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
~ # /etc/init.d/hostd restart
watchdog-hostd: Terminating watchdog process with PID 9592
hostd stopped.
hostd started.
~ #[/cc]

Finally, a quick check to see if the new Equallogic namespace is available, and that it is gathering statistics, i.e. being used:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
~ # esxcli equallogic stat summary
DeviceId VolumeName PathCount Reads Writes KBRead KBWritten
-------------------------------- ---------- --------- ----- ------ ------ ---------
6090A098E0DC5D9F71E6940292F8569C vmvolume 2 2573 30 20429 14
6090A098D06C5A31CEDE44CC17CBF14B test2t 2 651 30 13028 14
6090A098D06C4AF067EDD4C904C6A453 vmvolume3 2 642 30 10592 14
6090A098C08D5E928EE634938F42605B vmvolume1 2 1834 30 20023 14
~ #[/cc]





[![pre-mem-install](http://vninja.net/wordpress/wp-content/uploads/2013/04/pre-mem-install-300x221.png)](http://vninja.net/wordpress/wp-content/uploads/2013/04/pre-mem-install.png)

Screenshots displaying the ESXi host path policy before:[
](http://vninja.net/wordpress/wp-content/uploads/2013/04/pre-mem-install.png)











![post-mem-install](http://vninja.net/wordpress/wp-content/uploads/2013/04/post-mem-install-300x221.png)

and after installing the Dell Equallogic MEM:



[

](http://vninja.net/wordpress/wp-content/uploads/2013/04/post-mem-install.png)


