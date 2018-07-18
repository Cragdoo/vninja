---
author: Christian Mohn
comments: true
date: 2011-03-07 22:43:33+00:00
layout: post
slug: using-vma-as-a-local-vsphere-patch-repository
title: Using vMA as a local vSphere Patch Repository
url: /virtualization/using-vma-as-a-local-vsphere-patch-repository/
wordpress_id: 887
categories:
- Howto
- Virtualization
tags:
- ESX
- ESXi
- Ops
- vCLI
- Virtual Appliance
- vMA
- VMware
- vSphere
---

I like using http as the transport protocol when patching my vSphere hosts. It's easy to use and in most cases immediately available over most networks. Since I want to use http as the transport, we need to make vMA work as a http server.


### Starting Apache inside vMA


Luckily, the [Apache http daemon](http://www.apache.org/) is installed, by default, in vMA and to utilize it all you have to do is to start it!

Log on to vMA with your favorite SSH client and run the following command to start the Apache HTTP Daemon:

    
    [vi-admin@vMA /]$ sudo service httpd start
    Starting httpd: httpd: Could not reliably determine the server's fully qualified domain name, using 127.0.0.1 for  ServerName                   [ OK ]
    [vi-admin@vMA /]$


Never mind the error message it displays, for our purposes that's not an issue and we can safely ignore it.

By default the files served by Apache is located in _/var/www/html_, so we'll head over there to create a new directory

    
    [vi-admin@vMA /]$ cd /var/www/html/
    [vi-admin@vMA html]$ sudo mkdir repo
    


We've now created the repo directory inside the Apache docroot. Now we need to add some patches to that directory, to make  it available for the _vihostupdate_ or _esxupdate_ command we can use to patch our hosts.

In my lab, I used the **update-from-esxi4.1-4.1_update01** patch bundle from [vmware.com](https://hostupdate.vmware.com/software/VUM/OFFLINE/release-260-20110127-912579/update-from-esxi4.1-4.1_update01.zip)

To download the patch into the new repo directory created above, run the following commands:

    
    [vi-admin@vMA html]$ cd /var/www/html/repo/
    [vi-admin@vMA repo]$ sudo wget https://hostupdate.vmware.com/software/VUM/OFFLINE/release-260-20110127-912579/update-from-esxi4.1-4.1_update01.zip
    Password:
    --15:34:32--  https://hostupdate.vmware.com/software/VUM/OFFLINE/release-260-20110127-912579/update-from-esxi4.1-4.1_update01.zip
    	Resolving hostupdate.vmware.com... 88.221.164.7
    	Connecting to hostupdate.vmware.com|88.221.164.7|:443... connected.
    	HTTP request sent, awaiting response... 200 OK
    	Length: 215820281 (206M) [application/zip]
    	Saving to: `update-from-esxi4.1-4.1_update01.zip'
    100%[===============================================================================================================>] 
    
    215,820,281  919K/s   in 3m 54s
    15:38:26 (901 KB/s) - `update-from-esxi4.1-4.1_update01.zip' saved [215820281/215820281]
    


This downloads the patch bundle, using the wget command, to the current directory.

Now, to make sure your downloaded patch bundle is available via the web server, open **http://vMA-IP/repo/** and you should see the directory contents listed. Your browser should display something similar to this:

![](/img/vMA-Patch-1-300x117.png)

Before patching a host, power off or migrate any virtual machines that are running on the host and place the host into maintenance mode.


#### Scan host for update compatibility



    
    [vi-admin@vMA repo]$ vihostupdate --server 10.0.100.20 --scan --bundle http://10.0.101.14/repo/update-from-esxi4.1-4.1_update01.zip
    Enter username: root
    Enter password:
    The bulletins which apply to but are not yet installed on this ESX host are listed.
    
    ---------Bulletin ID---------   ----------------Summary-----------------
    ESXi410-201101201-SG            Updates the ESXi 4.1 firmware
    ESXi410-201101202-UG            Updates the ESXi  4.1 VMware Tools
    ESXi410-201101223-UG            3w-9xxx: scsi driver for VMware ESXi
    ESXi410-201101224-UG            vxge: net driver for VMware ESXi
    ESXi410-Update01                VMware ESXi 4.1 Complete Update 1
    





#### Install updates to host



    
    [vi-admin@vMA repo]$ vihostupdate --server 10.0.100.20 --install --bundle http://10.0.101.14/repo/update-from-esxi4.1-4.1_update01.zip
    Enter username: root
    Enter password:
    Please wait patch installation is in progress ...
    The update completed successfully, but the system needs to be rebooted for the changes to be effective.
    [vi-admin@vMA repo]$
    


While the update runs, you can also follow it's progress in the vSphere Client

[![](/img/vMA-Patch-2-300x247.png)](/img/vMA-Patch-2.png)
When the patch has completed, and the host has been rebooted you can run the scan command again to make sure all of the patches are installed and no longer required.


#### Management


Now, downloading the patches this way for each vMA instance you have (especially if you have several remote sites) is not very effective. Sure, you could place a central repository at a central site and use that as your central update repository. In that scenario you might as well just use the VMware vCenter Update Manager and not have to manage your updates via vMA at all.

In some cases though, you would want to have the remote hosts install their updates from a local repository. One such case might be if you have remote locations with low bandwidth/high latency links that you don't want to stress with the update downloads.

_I'm investigating a possible solution for that as well, and I'll post that as soon as I have working proof of concept up and running._

Another thing to note, is that when you restart vMA the http service will be stopped again. If you want it to autostart each time vMA boots, issue the following command

    
    [vi-admin@vMA repo]$ sudo ntsysv
    Password:
    


This brings up a screen where you can choose which daemons should start at boot time inside of vMA.

[![](/img/vMA-Patch-3-300x198.png)](/img/vMA-Patch-3.png)

Find _httpd_, select it and hit the OK button. The next time vMA boots, the Apache web server starts with it.
