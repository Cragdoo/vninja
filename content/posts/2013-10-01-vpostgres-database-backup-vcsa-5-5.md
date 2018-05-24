---
author: cmohn
comments: true
date: 2013-10-01 08:42:47+00:00
layout: post
slug: vpostgres-database-backup-vcsa-5-5
title: vPostgres Database Backup in vCSA 5.5
url: /virtualization/vpostgres-database-backup-vcsa-5-5/
wordpress_id: 2769
categories:
- Virtualization
- VMware
tags:
- Appliance
- Backup
- VCSA
- VMware
- vPostgres
- vSphere
---

With the new vSphere 5.5 release, the VMware vCenter Appliance (vCSA) has grown up to be a viable alternative to the traditional Microsoft Windows based vCenter deployment scenario. The new vCSA version supports up to 100 hosts and 3000 (with an external Oracle database the values change to 1000/10000) virtual machines, a big improvement from 5 hosts and 50 virtual machines in the previous version.

Sadly, the only external database option available for vCSA 5.5 is Oracle, which means there is still no external Microsoft SQL Server support. For those clients who don´t have an existing Oracle infrastructure, this might be a problem especially with regards to backup of the vCSA database.

The internal database in vCSA is a modified version of Postgres, that VMware has called vPostgres. Thankfully this also includes the native Postgres command to create database dumps, _pg_dump_.

In order to create a database dump of the vPostgres database, the following command needs to be run by opening a SSH connection to the vCSA:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
~ # /opt/vmware/vpostgres/1.0/bin/pg_dump EMB_DB_INSTANCE -U EMB_DB_USER -Fp -c > VCDBBackupFile
[/cc]

**EMB_DB_INSTANCE** and **EMB_DB_USER** are to be replaced with values from the _/etc/vmware-vpx/embedded_db.cfg_ file

In my case, the exact command would be:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
~ # /opt/vmware/vpostgres/1.0/bin/pg_dump VCDB -U vc -Fp -c > /tmp/VCDBBackupFile
[/cc]

Note that **EMB_DB_INSTANCE** is replaced with **VCDB** and **EMB_DB_USER** is replaced by vc in the command above. Both values come from _/etc/vmware-vpx/embedded_db.cfg_

Off it goes, and creates a dump file in _/tmp_. So far so good, we have a database dump but how to we automate it?

Add a new file in _/etc/cron.daily/_ (for instance, use _/etc/cron.hourly/_ if that fits your RPO) called _vcdbbackup.sh_ with the following content:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
#!/bin/sh
/opt/vmware/vpostgres/1.0/bin/pg_dump VCDB -U vc -Fp -c > /tmp/VCDBBackupFile
[/cc]

Then we need to make the cron script executable by running:
[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
chmod u+x /etc/cron.daily/vcdbbackup.sh
[/cc]

And you should be ready to go! Of course, you should probably create the backup files somewhere else than in _/tmp_, and even mount a file share from another server in your environment and place the dump file there for safe keeping and further backup in your existing scheme. In addition to this, you can add other variables to the script like datestamping the file etc, but for now this is what I have done.

By doing backups of the vCSA in this manner, you can have a standby vCSA laying around in case of a primary vCSA failure. If that happens, fire up the standby one, ssh to it and restore the vPostgres dump. I won't go into details on that right now, but the command for restoring looks like this:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
PGPASSWORD=EMB_DB_PASSWORD ./psql -d EMB_DB_INSTANCE -Upostgres -f VCDBBackupFile
[/cc]

Feature Request
_What VMware should do for the next version of the vCSA is to add external Microsoft SQL Server support, but if that´s off the table for some reason, at least let us manage database dumps via the vCSA admin interface? Please create pre-defined scripts and crontabs, and let us manage those. It might not be as good as external database support, when it comes to backup, but a little goes a long way in protecting this valuable resource in your infrastructure._


#### References:




#### [Backing up and restoring the vCenter Server Appliance (vPostgres) database (2034505)
](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2034505)
