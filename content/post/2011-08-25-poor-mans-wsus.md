---
author: Christian Mohn
comments: true
date: 2011-08-25 21:51:37+00:00
layout: post
slug: poor-mans-wsus
title: Poor Man's WSUS
url: /microsoft-2/poor-mans-wsus/
wordpress_id: 1384
categories:
- Microsoft
tags:
- Microsoft
- Ops
- Patching
- Tool
- WSUS
---

Not that WSUS is expensive, after all it's a "free" addon to the server you're already running if you need it. Of course, running your own [Windows Server Update Services](http://technet.microsoft.com/en-us/windowsserver/bb332157) (WSUS) infrastructure is preferable in just about every scenario except for some edge cases where bandwidth or latency issues might prevent you from syncing the updates from a central repository. Sadly, these edge cases enter the fray from time to time and I recently found myself in the middle of such a scenario. Without a WSUS infrastructure to sync from, and with very poor network connectivity how would you go about getting hold of all the Microsoft patches for a given OS and Office suite? Manually download all the patches, and then manually (or scripted) apply them to the clients?

Thankfully his is where [WSUS Offline Update](http://www.wsusoffline.net/) comes to the rescue! This clever little application, that requires no installation what so ever, helps you patch Microsoft Windows and Microsoft Office without having an active network connection.

In essence WSUS Offline Update lets you specify which operating systems and what versions of Office (and languages) you want to have patches available for in it's repository. Run _..\wsusoffline\UpdateGenerator.exe_ to select versions and download them.

[![Poor Mans WSUS Screenshot](/img/PoorMansWsus02-150x150.png)](/img/PoorMansWsus02.png)[![Poor Mans WSUS Screenshot](/img/PoorMansWsus03-150x150.png)](/img/PoorMansWsus03.png)











It then proceeds to connect to Microsoft and download all applicable patches, and service packs if you want it to, figuring out the dependencies and supersedes along the way.

[![](/img/PoorMansWsus05-150x150.png)](/img/PoorMansWsus05.png)











Of course, this takes a while, but once it's done you have a lovely little local repository of all the current Microsoft patches. Since WSUS Offline Update doesn't require an installation, just unzip and run, it's also very easy to move the repository to an external drive, USB stick or even transport it via the network. In addition it also provides built in methods for creating ISO files or place the files directly on a USB media.

When the patches have been downloaded, all you have to do is get the files transferred to the target client and run the _..\wsusoffline\client\UpdateInstaller.exe_ file and the patches will be applied to the system you ran it on.

It also keeps track of it's local repository, to make sure that the next time it's run it only downloads patches that has been released by Microsoft since the last run. All in all, this a great tool to keep in your tool belt, it sure helped me this time!
