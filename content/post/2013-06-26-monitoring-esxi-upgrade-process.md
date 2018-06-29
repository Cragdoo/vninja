---
author: cmohn
comments: true
date: 2013-06-26 10:34:18+00:00
layout: post
slug: monitoring-esxi-upgrade-process
title: Monitoring the ESXi Upgrade Process
url: /vmware-2/monitoring-esxi-upgrade-process/
wordpress_id: 2615
categories:
- VMware
tags:
- ESXi
- Log
- Maintenance
- Ops
- Real World
- Upgrade
- Virtualization
- VMware
- vSphere
---

When doing manual host upgrades, either through [the direct method](http://vninja.net/vmware-2/quick-dirty-esxi-5-1u1-upgrade/) or via a locally placed upgrade bundle, there is a distinct lack of progress information available after running the esxcli command. 

Thankfully the ESXi host provides a running logfile of the upgrade process, which makes it much easier to keep track of what is going on and that the upgrade is indeed being performed.

The _esxupdate.log_ is located in _/var/log_, and by issuing the following command in a terminal window you can have a rolling log showing you the upgrade status and progress:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
~ # tail -f /var/log/esxupdate.log
[/cc]

By running the following command in one terminal window (this uses the VMware offline bundle to upgrade from ESXi 5.1 to 5.1 Update 1):

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
~ # esxcli software vib update -d /vmfs/volumes/[lunID]/update-from-esxi5.1-5.1_update01.zip
[/cc]

you get output like this in the secondary terminal window where the log file is being monitored:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
2013-06-26T10:24:51Z esxupdate: HostImage: INFO: Attempting to download VIB tools-light
2013-06-26T10:25:07Z esxupdate: vmware.runcommand: INFO: runcommand called with: args = '/sbin/backup.sh 0 /altbootbank', outfile = 'None', returnoutput = 'True', timeout = '0.0'.
2013-06-26T10:25:08Z esxupdate: BootBankInstaller.pyc: INFO: boot config of '/altbootbank' is being updated, 5
2013-06-26T10:25:08Z esxupdate: HostImage: DEBUG: Host is remediated by installer: locker, boot
2013-06-26T10:25:08Z esxupdate: root: DEBUG: Finished execution of command = vib.install
2013-06-26T10:25:08Z esxupdate: root: DEBUG: Completed esxcli output, going to exit esxcli-software
[/cc]

That sure beats waiting "blindly" for an upgrade/installation to finish, and in many ways this is also much better than a non-sensical progressbar.
