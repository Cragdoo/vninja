---
author: cmohn
comments: true
date: 2012-09-17 21:24:18+00:00
layout: post
slug: vmware-esxi-4-vs-esxi-5-log-file-locations
title: VMware ESXi 4 vs ESXi 5 Log File Locations
url: /virtualization/vmware-esxi-4-vs-esxi-5-log-file-locations/
wordpress_id: 1490
categories:
- Virtualization
- VMware
tags:
- ESXi
- ESXi 4
- ESXi 5
- Log Files
- Ops
- VMware
- vSphere
---

This post is inspired by a [tweet](http://twitter.com/#!/astorrs/status/113710978868326400) from [Andrew Storrs](http://twitter.com/astorrs), where he pinpoints that the host log file locations have changed between ESXi 4 and ESXi 5.

**Note: This post has been updated with new log files for ESXi 5.1**


### ESXi 4 Log File Locations:


[table id=6 /]


### ESXi 5 Log File Locations:


[table id=5 /]

Between version 5.0 and 5.1 the log file locations have not changed, but a couple of new logs have been added.





### ESXi 5.1 New Log File Locations:






[table id=10 /]




Clearly the number of host log file has increased in newer versions, and that should make it much easier to find the log entries you are looking for. A more granular logging into different specialized log files can only be a good thing.





### Logs from vCenter Server Components on ESXi 5.1:






[table id=11 /]






<del>Just remember that ESXi only logs to memory, and that you need to set logging to a syslog server to preserve logs between reboots.</del> If ESXi 5 is installed on local disk, the log files will be [persistent through reboots](http://vninja.net/virtualization/vmware-esxi-4-vs-esxi-5-log-file-locations/#comment-3971), since it creates a zipped archive in **/var/run/log**. If ESXi is deployed via Auto Deploy, no local disk is used and [the log files are not persistent](http://vninja.net/virtualization/vmware-esxi-4-vs-esxi-5-log-file-locations/#comment-3973), and needs to be collected by an external syslog service.

For more details about various VMware products and their log file locations, check [VMware Knowledgebase article 1021806](http://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=1021806) and [VMware Knowledgebase article 2032076](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2032076)
