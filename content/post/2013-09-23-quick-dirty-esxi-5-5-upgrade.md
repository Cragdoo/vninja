---
author: Christian Mohn
comments: true
date: 2013-09-23 23:28:04+00:00
layout: post
slug: quick-dirty-esxi-5-5-upgrade
title: Quick and Dirty ESXi 5.5 Upgrade
url: /virtualization/quick-dirty-esxi-5-5-upgrade/
wordpress_id: 2728
categories:
- Virtualization
tags:
- '5.5'
- Command Line
- esxcli
- ESXi
- Host
- Upgrade
- VIB
- VMware
- vSphere
---

![UpgradeESXi](http://vninja.net/wordpress/wp-content/uploads/2013/09/UpgradeESXi-150x150.png)As I´ve posted about earlier, you can [update your ESXi hosts](http://vninja.net/vmware-2/quick-dirty-esxi-5-1u1-upgrade/) to a new release from the command line. Now that [ESXi 5.5](https://www.vmware.com/support/vsphere5/doc/vsphere-esx-vcenter-server-55-release-notes.html?utm_content=buffer6215e&utm_source=buffer&utm_medium=twitter&utm_campaign=Buffer) has been released, the same procedure can be applied to upgrade once more.

Place the host in maintenance mode, then run the following command to do an online update to ESXi 5.5:

{{< highlight bash >}}
~ # esxcli software profile update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml -p ESXi-5.5.0-1331820-standard
{{< /highlight >}}

While this runs, [monitor the log file](http://vninja.net/vmware-2/monitoring-esxi-upgrade-process/) to check upgrade process:

{{< highlight bash >}}
~ # tail -f /var/log/esxupdate.log
{{< /highlight >}}

Let it run for a while all the way until it´s finished, reboot the host and hey presto, fresh new ESXi 5.5 upgrade completed! Quick and dirty, just the way we like it.

To verify the version, still using the command line, issue the following command:

{{< highlight bash >}}
~ # vmware -l
VMware ESXi 5.5.0 GA
{{< /highlight >}}
** Of course, test this thoroughly before doing this in a production environment, after all your hosts might need VIBs not included in the standard download.**



### Update 16.02.2015



To find out which patches to apply, and what the correct profile name would be, check out the [VMware ESXi Patch Tracker](http://esxi-patches.v-front.de) by [Andreas Peetz](http://twitter.com/VFrontDe). It's a great resource to keep track of patches for your ESXi hosts.
