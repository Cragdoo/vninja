---
author: cmohn
comments: true
date: 2010-10-04 21:36:23+00:00
layout: post
link: http://vninja.net/virtualization/vcenter-4-1-database-recovery-model-defaults/
slug: vcenter-4-1-database-recovery-model-defaults
title: vCenter 4.1 Database Recovery Model Defaults
wordpress_id: 316
categories:
- Virtualization
tags:
- Backup
- Maintenance
- Ops
- SQL Server Express
- vCenter
- vSphere
---

![](/images/logos/vmware-logo.gif)Sometimes leaving the defaults in place might just come back and bite you, hard.

That might also be the case with your vCenter 4.1 database, as I experienced back in July.

All of a sudden my vCenter Server stopped working. The symptoms where pretty obvious, my client couldn't connect to the vCenter server. Naturally I connected to the server, and noticed that the VMware VirtualCenter Server services had indeed stopped. No wonder the client couldn't connect to it. I tried starting it, but if would shut itself down again after a few seconds.

The next step, obviously, was to look at the event logs and there it was, in plain English explaining the service desire to stop:

    
    Log Name:      Application
    Source:        MSSQL$SQLEXP_VIM
    Date:          17.07.2010 02:06:16
    Event ID:      9002
    Task Category: (2)
    Level:         Error
    Keywords:      Classic
    User:          SYSTEM
    Computer:      [removed]
    Description:
    The transaction log for database 'VIM_VCDB' is full. To find out why space in the log cannot be reused, see the log_reuse_wait_desc column in sys.databases


Important bit:
**The transaction log for database 'VIM_VCDB' is full. To find out why space in the log cannot be reused, see the log_reuse_wait_desc column in sys.databases**

As it turns out, the transaction log for my SQL Server Express based vCenter install database was indeed full. Not only was it full, it had grown to eat just about each and every byte out of the disk space on the server.

I got around this by changing the recovery model back to simple, and performing a new backup via the script I've mentioned earlier: [Scheduling vCenter Backups](http://vninja.net/virtualization/scheduling-vcenter-backups/). I'm not saying this is how you should configure your own vCenter recovery model, you need to chose one that fits your specific backup scheme. Just make sure you understand how the different settings affect your particular environment.

So, where does the defaults come into play?
As VMware explains in [KB Article 1001046 : SQL Server Recovery Model Affects Transaction Log Disk Space Requirements](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1001046&sliceId=1&docTypeID=DT_KB_1_1&dialogID=115184631&stateId=0%200%20120616083) the install defaults to the **Bulk Recovery** model. This model allows the transaction log to grow, until a backup clears it and lets it start over. If you, like me, used a scripted backup that doesn't do transaction log pruning you might very well be in the same predicament pretty soon.

_So, take my advice, make sure you understand the backup routines you have in place for your vCenter and check those log settings before your vCenter stops unexpectedly. _

**Do yourself a favour, check your log files and do it now.**

Of course, this is not the only thing that has an effect on your vCenter Database size. The settings for [vCenter Server Statistics](http://www.vcritical.com/2009/04/vmware-vcenter-server-performance-stats-levels/) also comes into play, but in my case the issue was the backup scheme I had in place and the default settings for a new vCenter 4.1 install.
