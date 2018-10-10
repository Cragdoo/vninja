---
author: Christian Mohn
comments: true
date: 2010-07-21 12:11:28+00:00
layout: post
slug: migrating-your-vcenter-from-32bit-to-64bit
title: Migrating your vCenter from 32bit to 64bit
url: /virtualization/migrating-your-vcenter-from-32bit-to-64bit/
wordpress_id: 63
categories:
- Virtualization
tags:
- Maintenance
- Ops
- Upgrade
- vCenter
- VMware
---

![](/images/logos/vmware-logo.gif) As you may or may not know, the new vCenter 4.1 requires that the host it runs on is 64 bit. As 4.0 and previous versions weren't supported on 64 bit at all, this probably means that when you upgrade you will need to move your existing database to a new host.

There are several ways of doing the migration, but one way is to backup your existing database, and restore it on a new host and point the new vCenter 4.1 installation to the existing database and have it upgrade it for you during install.

I'm sure that you would like to automate that process, just to limit the possibility of manual screw ups, right? Thankfully, VMware had the same idea.

In chapter 5 of the [vSphere Upgrade Guide](http://www.vmware.com/pdf/vsphere4/r41/vsp_41_upgrade_guide.pdf), VMware outlines the different methods you can use when migrating your vCenter database, and on page 39 a data migration tool makes it's appearance.

This two step process takes a backup of your existing vCenter SQL Server Express database and dumps it to a predefined local folder. You then copy the entire migration tool folder to the new host and run the second part of the tool to install vCenter 4.1 and import the old database.

I used this tool to migrate our setup, from Windows Server 2003 32bit to Windows Server 2008 R2 64bit without problems.

**The only thing you need to be aware of is that [VMware Update Manager is still a 32bit application](http://www.yellow-bricks.com/2010/07/15/32-bit-odbc-dsn-for-vum-4-1/) and requires that you manually create a 32bit DSN to the database. This is mentioned in the [VUM Installation and Administration Guide](http://www.vmware.com/pdf/vsp_vum_41_admin_guide.pdf), but it is not mentioned in the vSphere Upgrade Guide.
**

All in all, this tool is a great assistant for all of you that need to migrate to a new host for 64bit support in vCenter Server 4.1. While it could be more polished, brand a GUI and let you use network storage for the backups, it does the job at hand very well. After all, who needs a polished GUI based application for a one off job like this?

#### Related VMware Knowledgebase Articles

  * [vSphere 4.1 upgrade pre-installation requirements and considerations](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1022137&sliceId=1&docTypeID=DT_KB_1_1&dialogID=90426222&stateId=1%200%2090428570)
  * [Migrating an existing vCenter Server database to 4.1 using the data migration tool](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1021635&sliceId=1&docTypeID=DT_KB_1_1&dialogID=90426222&stateId=1%200%2090428570)
  * [vCenter Server 4.1 Data Migration Tool fails with the error: HResult 0x2, Level 16, State 1](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1024490)
  * [Using the Data Migration Tool to upgrade from vCenter Server 4.0 to vCenter Server 4.1 fails](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1024380&sliceId=1&docTypeID=DT_KB_1_1&dialogID=90426222&stateId=1%200%2090428570)