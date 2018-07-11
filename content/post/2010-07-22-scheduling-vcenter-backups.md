---
author: Christian Mohn
comments: true
date: 2010-07-22 12:17:56+00:00
layout: post
slug: scheduling-vcenter-backups
title: Scheduling vCenter Backups
url: /virtualization/scheduling-vcenter-backups/
wordpress_id: 91
categories:
- Virtualization
tags:
- Backup
- Ops
- SQL Server Express
- vCenter
- VMware
---

![](/images/logos/vmware-logo.gif)If you run your vCenter on SQL Server Express 2005, you are missing the ability to set up scheduled backup jobs with SQL Maintenance Plans, a feature available in the full version of SQL Server. 

This might not be a problem if your backup software has SQL Server agents that you use to backup your vCenter databaser, but in smaller environments or even in your lab, you might not have that kind of backup scheme available to you, so what do you do? Thankfully there are ways of setting up the same kind of scheduled backups in SQL Server Express, without being a SQL Server Guru.



#### Creating a Backup Script


If you don't have [SQL Server Management Studio Express](http://www.microsoft.com/downloads/details.aspx?familyid=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&displaylang=en) installed already, download and install it now.  

Fire it up and log on with a user that has sufficient permissions to access the vCenter database  

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/1-300x224.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/1.png)

Find your vCenter database by expanding _Databases_ and select **VIM_VCDB**  


[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-2-300x246.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-2.png)

Right click on **VIM_VCDB** and select _Tasks_ and then _Back Up..._  


[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-3-300x279.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-3.png)

This opens the Back Up Database window, where you set your backup options. Set your options in a manner that fits your environment. You can set options like the backup file location, retention policy etc.  


[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-4-300x236.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-4.png)

So far, this is all fine and dandy. You can create a manual backup this way, without much hassle. How can we turn that into a scheduled job?  

The first bit is to turn your backup options into a SQL script that can be scheduled. You do this by finding the _Script_ drop-down menu on the top of the Backup Database window. Select _Script Action to New Query Window_.  


[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-5-300x236.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-5.png)

This opens the script window, where you can see the script and test it to make sure it works as intended.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-6-300x225.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-6.png)

The next step is to save the generated script, you do so my going to _File_ and select **Save ... As**. I've created a folder called _c:\scripts\_ that I use to store my SQL scripts in, so I'll save the backup script there as _FullBackupVCDB.sql_.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-61-300x245.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-61.png)



#### Scheduling the Backup Script



Now that we have a working backup script, we need to be able to schedule it to run. As we can't do that within the SQL Server Management Studio Express application, we need to find another way of scheduling it. Windows Server 2008 R2 (and other versions) include a scheduling tool, and that's what we'll use to create our schedule.

On my standard vCenter installation, the SQL Server is installed in the default location of _C:\Program Files (x86)\Microsoft SQL Server\_. This means that the actual command we need to schedule will be (be sure to replace the server-/instance-name and script name if your values differ from mine):   


_"C:\Program Files (x86)\Microsoft SQL Server\90\Tools\Binn\SQLCMD.EXE" -S [servername]\SQLEXP_VIM -i c:\scripts\FullBackupVCDB.sql_
 
Go to the Control Panel and select Schedule Tasks. Click **Create Basic Task**, give it a name and set an appropriate schedule.  

Select **Start a program** as the action for the task, and when it asks for **Program/Script** enter _"C:\Program Files (x86)\Microsoft SQL Server\90\Tools\Binn\SQLCMD.EXE" -S [servername]\SQLEXP_VIM -i c:\scripts\FullBackupVCDB.sql_.

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-7-300x212.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-7.png)

Click next and check the box that says **Open the Properties dialog for this task when I click Finish** then click _Finish_. In the VCDB Backup properties, make sure the **Run whether user is logged on or not** option is selected, to make sure the schedule runs as planned.

**Once you have verified that the schedule works as intended, make sure that you include the location for your vCenter database in your regular backup scheme, and you should we a lot safer than you were.**

[![](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-8-300x112.png)](http://vninja.net/wordpress/wp-content/uploads/2010/07/Scheduling-vCenter-Backups-8.png)

**That's it!  Scheduled vCenter backups on SQL Server Express 2005.**

Thanks to [Chris Dearden](http://twitter.com/ChrisDearden) over at  [J.F.V.I](http://jfvi.co.uk/) who helped me out with getting my _sqlcmd.exe_ syntax corrected for the scheduled task!
