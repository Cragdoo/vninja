---
author: cmohn
comments: true
date: 2015-03-13 11:34:21+00:00
layout: post
slug: vmware-software-manager-changing-download-directory
title: VMware Software Manager - Changing Download Directory
url: /virtualization/vmware-software-manager-changing-download-directory/
wordpress_id: 3658
categories:
- Virtualization
tags:
- VMware
- VMware Software Manager
- vSphere 6
---

The new [VMware Software Manager](http://blogs.vmware.com/vsphere/2015/03/downloading-vmware-suite-push-button-using-vmware-software-manager.html), which was released at the same time as vSphere 6, is a great way to get your download ducks in a row, and not manually download all the different vSphere pieces one by one.

But all these downloads sure eat up disk space, and if you, like me, chose the wrong download location while installing VMware Software Manager what do you do? <del>There is no way in the web interface to change the download directory after installation, so how do you change it? </del>There is a way through the GUI as well, but here is how to do it manually:

<!--more-->


It turns out that the configuration is stored in **%appdata%\VMware\Software Manager\Download Service\Cfg\application.cfg**, specifically under the **[download]** section. To change your download location, move your existing directory to a new location, and update the path under **directory=**. Then use Task Manager to kill the **vsm_download_service.exe** task(s) that are running, and restart it from the **C:\Program Files\VMware\Software Manager\Download Service** directory and you should be all set to download to the new location.



### Update:

It turns out, that you can in fact change this setting through the GUI as well, just not via the web interface. If you right click the VMware Software Manager icon in your tray, you can change the location by choosing the **Settings->Change Download Location** option. I completely overlooked that when trying to change the location at first.
