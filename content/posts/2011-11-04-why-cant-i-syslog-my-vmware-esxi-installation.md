---
author: cmohn
comments: true
date: 2011-11-04 22:06:32+00:00
layout: post
slug: why-cant-i-syslog-my-vmware-esxi-installation
title: Why Can't I Syslog my VMware ESXi Installation?
url: /virtualization/why-cant-i-syslog-my-vmware-esxi-installation/
wordpress_id: 1601
categories:
- Virtualization
- VMware
tags:
- ESXi
- ESXi 5
- Feature Request
- Installation
- Syslog
---

[Juan Manuel Rey](https://twitter.com/#!/jreypo)'s post [Monitor ESX 4.x to ESXi 5.0 migration process](http://jreypo.wordpress.com/2011/11/04/monitor-esx-4-x-to-esxi-5-0-migration-process/) show how you can watch the progress of an ESX 4 to ESXi 5 upgrade procedure, by looking at the live logs.

While this is very useful, and in many cases a real learning experience, it got me thinking that these logs should be available remotely as well. Since ESXi supports, and actively encourages, the use of an external [Syslog](http://en.wikipedia.org/wiki/Syslog) service for log file safekeeping and monitoring, shouldn't the installation logs for ESXi also be logged externally if configured?

Thinking that I couldn't be the first person that thought of this, I looked through the scripting section of [vSphere Installation and Setup - vSphere 5.0 guide](http://pubs.vmware.com/vsphere-50/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-50-installation-setup-guide.pdf) I was very surprised to see that there is no option to configure syslogging until after the installation is finished and the host configuration script(s) runs (ks.cfg).

By using a _ks.cfg_ script you can automatically configure syslog settings, but since that happens after the installation is done, and the host is potentially rebooted, the installation logs are lost (ESXi logs are not persistent by default) unless you run something that copies them over to another location before the reboot happens.

Of course, when [I asked on Twitter](https://twitter.com/#!/h0bbel/status/132565917732319232) the [Godfather of Ghetto](http://www.virtuallyghetto.com/), [William Lam](http://www.twitter.com/lamw) [responded](https://twitter.com/#!/lamw/status/132570077814996992) that you could always create a python script that runs after the install, before a reboot, that uploads the logs to a syslog server. While this is all fine and dandy, I would still like to have the possibility to configure a syslog server during the installation, and have the installation procedure fling all it's log goodness at the syslog server while the installation runs.

With new features like Autodeploy being utilized, having these logs automatically gathered (without ghetto hacking the installation) in a central location sounds like a really good idea to me? 

**Surely, I'm not the only one? Is this something that has enough brains behind it to actually warrant a proper feature request being filed with VMware?**
