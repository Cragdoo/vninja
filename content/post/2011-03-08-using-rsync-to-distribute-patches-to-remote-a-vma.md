---
author: cmohn
comments: true
date: 2011-03-08 08:00:02+00:00
layout: post
slug: using-rsync-to-distribute-patches-to-remote-a-vma
title: 'Using rsync to Distribute Patches to a Remote vMA '
url: /virtualization/using-rsync-to-distribute-patches-to-remote-a-vma/
wordpress_id: 902
categories:
- Howto
- Virtualization
tags:
- ESX
- ESXi
- Ops
- rsync
- vCLI
- Virtual Appliance
- vMA
- VMware
- vSphere
---

I recently posted [Using vMA as a local vSphere Patch Repository](http://vninja.net/virtualization/using-vma-as-a-local-vsphere-patch-repository/), where I outlined how you can use your vMA instances as local file repositories for updates.

This post is a continuation of that concept, but this time I'll take it a step further and utilize [rsync](http://samba.anu.edu.au/rsync/)  to make sure my vMA instances all contain the same set of patches. Rsync is great for this, as it handles fast incremental file transfers, which is a real time and bandwidth saver in my particular scenario. So, the premise is that you have one central vMA instance, and one or more remote vMA instances that should pull updates from the centrally located one.



### Installing rsync in vMA


Sadly, rsync isn't included in vMA by default. To get it installed, we need to edit some files inside of vMA. Since vMA is CentOS based, this means configuring [yum](http://www.centos.org/docs/5/html/yum/) repositories, and thankfully the brilliant [William Lam](http://twitter.com/lamw) over at [virtuallyGhetto](http://www.virtuallyghetto.com) has already done the hard work for us. In his post named [Automate Update Manager Operations using vSphere SDK for Perl + VIX + PowerCLI + PowerCLI VUM](http://www.virtuallyghetto.com/2010/07/automate-update-manager-operations.html) William explains which files to edit to create a valid repository configuration for installation of official packages directly from CentOS.

_Warning: Do this on your own risk, I have not checked but I think this will fall under the "unsupported" tab at VMware_.



#### Configuring YUM in vMA


These instructions are pretty much copied from Williams post, but added here for context:



##### Creating the repository configuration file


To create the file, in the correct directory, run the followinf command:

    
    [vi-admin@vMA /]$ sudo vi /etc/yum.repos.d/centos-base.repo
    Password:
    


Add the following lines to the repository file:

    
    
    [base]
    name=CentOS-5 - Base
    baseurl=http://mirror.centos.org/centos/5/os/x86_64/
    gpgcheck=1
    gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-5
     
    [update]
    name=CentOS-5 - Updates
    baseurl=http://mirror.centos.org/centos/5/updates/x86_64/
    gpgcheck=1
    gpgkey=http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-5
    



Exit the **vi** editor by hitting _esc_ and entering _:wq_ and hit enter. That saves the file, and exits the editor.



##### Installing rsync via yum


Now comes the easy part, actually installing rsync inside vMA. All you have to do, is to enter the following command:

    
    
    [vi-admin@vMA /]$ sudo yum -y install rsync
    


The installation starts, and you should see output similar to the following:

    
    
    Loaded plugins: fastestmirror
    Loading mirror speeds from cached hostfile
    Setting up Install Process
    Parsing package install arguments
    Resolving Dependencies
    --> Running transaction check
    ---> Package rsync.x86_64 0:2.6.8-3.1 set to be updated
    --> Finished Dependency Resolution
    
    Dependencies Resolved
    
    ====================================================================================
     Package           Arch               Version                Repository        Size
    ====================================================================================
    Installing:
     rsync             x86_64             2.6.8-3.1              base             235 k
    
    Transaction Summary
    ====================================================================================
    Install      1 Package(s)
    Update       0 Package(s)
    Remove       0 Package(s)
    
    Total download size: 235 k
    Downloading Packages:
    rsync-2.6.8-3.1.x86_64.rp 100% |=========================| 235 kB    00:01
    Running rpm_check_debug
    Running Transaction Test
    Finished Transaction Test
    Transaction Test Succeeded
    Running Transaction
      Installing     : rsync                                             [1/1]
    
    Installed: rsync.x86_64 0:2.6.8-3.1
    Complete!
    [vi-admin@vMA /]$
    



And there it is, rsync installed inside vMA!



### Configuring rsync to fetch upgrades from central vMA


Now that we have rsync installed inside vMA, we need to configure it to fetch the updates from a central vMA instance. Rsync needs to be installed in both ends of the pipe, so if you haven't already done so, configure your "master vMA" the same way as mentioned above.

Now that "both ends" of the pipe has rsync installed, we can run it from "client vMA" to pull down all the files currently in the repository on the "master vMA".

    
    
    [vi-admin@vMA /]$sudo rsync -r vi-admin@192.168.5.57:/var/www/html/repo/* /var/www.html/repo
    The authenticity of host '192.168.5.57 (192.168.5.57)' can't be established.
    RSA key fingerprint is 3f:af:7c:53:5a:15:47:56:b8:25:77:79:14:b2:f5:2f.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '192.168.5.57' (RSA) to the list of known hosts.
    vi-admin@192.168.5.57's password:
    [vi-admin@vMA /]$
    


The command runs for a while, and when it finished you should see that the current contents of the "master vMA" repository is now located in the "client vMA" repository as well:

    
    
    [vi-admin@vMA repo]$ ls -la
    total 210988
    drwxr-xr-x 2 root root      4096 Mar  8 18:51 .
    drwxr-xr-x 4 root root      4096 Mar  8 18:50 ..
    -rw-r--r-- 1 root root         0 Mar  8 18:51 testfile
    -rw-r--r-- 1 root root         0 Mar  8 18:51 testfile1
    -rw-r--r-- 1 root root         0 Mar  8 18:51 testfile2
    -rw-r--r-- 1 root root         0 Mar  8 18:51 testfile3
    -rw-r--r-- 1 root root 215820281 Jan 27 19:15 update-from-esxi4.1-4.1_update01.zip
    [vi-admin@vMA repo]$
    





#### Conclusion


There is a lot more you can do with rsync, like replication files both ways, controlling bandwidth usage, using ssh keys to avoid username/password prompts, something that is required if you want to fully automated this process. I will not cover that, at least not right now, so head over to the [rsync](http://samba.anu.edu.au/rsync/) site to read up on the documentation for more advanced use cases.


Even if I've barely touched the features rsync provides, it is clear that this is a way for admins to centrally manage distribution of vSphere patches to remote locations, even if the bandwidth is low and the latency high. Rsync provides us with ways to overcome the patching issues that you might see in poorly networked environments, and it can certainly help vAdmins keeping their environments patched and current, and that has to be a good thing™
