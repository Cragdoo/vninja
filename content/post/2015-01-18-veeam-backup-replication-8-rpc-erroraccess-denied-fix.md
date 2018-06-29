---
author: cmohn
comments: true
date: 2015-01-18 22:57:34+00:00
layout: post
slug: veeam-backup-replication-8-rpc-erroraccess-denied-fix
title: 'Veeam Backup & Replication 8: RPC error:Access is denied Fix'
url: /virtualization/veeam-backup-replication-8-rpc-erroraccess-denied-fix/
wordpress_id: 3497
categories:
- Virtualization
tags:
- Backup
- Troubleshooting
- Veeam
---

I recently set up a new [Veeam Backup & Replication v8](http://www.veeam.com/vm-backup-recovery-replication-software.html) demo lab, and my intial small job that consisted of two different Linux VMs and one Windows Server 2012 R2 Domain Controller was chugging along nicely. I had one minor from the start though, and that was that file indexing consistently failed for the Windows VM. No big deal, but I thought it was strange at the time. After all, the Linux VMs were indexed just fine. [![Veeam-1](http://vninja.net/wordpress/wp-content/uploads/2015/01/Veeam-1-300x272.png)](http://vninja.net/wordpress/wp-content/uploads/2015/01/Veeam-1.png)

Fast forward a few days, and all of a sudden Veeam B&R was unable to back up the Windows VM at all, failing with the following error:



[cc width="100%" theme="blackboard" nowrap="0"]
18.01.2015 19:48:01 :: Processing Joey Error: Failed to check whether snapshot is in progress (network mode).
RPC function call failed. Function name: [IsSnapshotInProgress]. Target machine: [192.168.5.5].
RPC error:Access is denied.
Code: 5
[/cc]

Nothing had changed in my environment, no patches had been installed, no changes made to the backup job or the credentials used. I even tried deleting the job, this is a demo environment after all, and re-creating it, but with the same end result.

As the **Access is denied** message clearly states, this had to be related to permissions somehow, but I was using domain administrator credentials (again, this is a _lab_), so all the required permissions should be in place, and the credentials test in the backup job also checked out just fine. It has also worked fine for 5 or 6 days, so I was a bit baffled.

In the end, I tried changing from using **User Principle Name (UPN)** connotation of _administrator@domain.local_ in the credentials for the VM, to using **Down-Level Logon Name** aka _domain\administrator_ and retried the job.

[![Veeam-2](http://vninja.net/wordpress/wp-content/uploads/2015/01/Veeam-2-300x272.png)](http://vninja.net/wordpress/wp-content/uploads/2015/01/Veeam-2.png)**That did the trick, and it also fixed the indexing problems I've had since setting up the job in the first place.**

According to this [Veeam Community Forum thread](http://forums.veeam.com/vmware-vsphere-f24/rpc-function-call-failed-rpc-error-acces-is-denied-t24111.html) this was also a problem in v7 and has something to do with how the Microsoft API's work.

So, if your Veeam Backup & Replication jobs fail with access denied messages, and/or can't index the VM files, check your credentials. They may work, but they _might_ just be entered in the wrong format.
