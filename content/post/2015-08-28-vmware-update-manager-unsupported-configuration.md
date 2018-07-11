---
author: cmohn
comments: true
date: 2015-08-28 10:46:24+00:00
layout: post
slug: vmware-update-manager-unsupported-configuration
title: 'VMware Update Manager: Unsupported Configuration'
url: /vmware-2/vmware-update-manager-unsupported-configuration/
wordpress_id: 3769
categories:
- VMware
tags:
- ESXi
- NFS
- Upgrade
- vCenter
- Veeam Backup &amp; Replication
- VMware
---

During an upgrade from vSphere 5.1 to 5.5, I ran into a rather strange issue when trying to utilize VMware Update Manager to perform the ESXi upgrade.

During scanning, VUM reported the ESXi host as "Incompatible", without offering any other explanation. I spent ages looking through VUM logs, trying to find the culprit, suspecting it was an incompatible VIB. Without finding anything that gave me any indication on what the problem might be, I moved on to looking at the ESXi image I had imported into VUM.

<!--more-->


As this was on a Dell PowerEdge R710, I was utilizing the Dell Customized Image of VMware ESXi 5.5 Update 2, which got an updated A02 version last night (27th of August) - I downloaded my image, **VMware-VMvisor-Installer-5.5.0.update02-2068190.x86_64-Dell_Customized-A00.iso** on the 27th, but before the **VMware-VMvisor-Installer-5.5.0.update02-2068190.x86_64-Dell_Customized-A02.iso** image was available. Thinking this would resolve the issue, I imported the new image into VUM, and created a new Upgrade Baseline. Sadly, I was still greeted by the non-gracious "Incompatible" warning after performing a new scan.

After some more digging, I found the following entry in the Events pane, for the given host, in vCenter:
{{< highlight bash >}}
Error in ESX configuration file (esx.conf).
error
28.08.2015 11:51:25
Scan entity
[hostname]
[username]
{{< /highlight >}}

Naturally I go digging into the  _/etc/vmware/esx.conf_ file, and found the following entries:

[{{< highlight bash >}}
/nas/[oldserver]/readOnly = "false"
/nas/[oldserver]/enabled = "true"
/nas/[oldserver]/host = "[oldserver.fqdn]"
/nas/[oldserver]/share = "/VeeamBackup_[oldserver]/"
{{< /highlight >}}


These references to [oldserver] pointed to an old Veeam Backup & Replication server that was decommissioned ages ago. Veeam B&R adds these to a host if vPowerNFS has been used to mount a backup share, and the entries had not been removed when removing the old Veeam B&R server. DNS resolution for the old server failed, as it has been completely removed from the infrastructure, thus causing the VUM update to fail.

Manually removing these lines from _/etc/vmware/esx.conf_ fixed the problem, and VUM was able to scan and remediate without issues.



##### Update:



After writing this, I saw [Jim Jones](http://twitter.com/k00laidIT) had the same experience, for more details [read his post:
Unsupported Configuration when using VUM for a Major Upgrade](https://koolaid.info/unsupported-configuration-using-vum-major-upgrade/)
