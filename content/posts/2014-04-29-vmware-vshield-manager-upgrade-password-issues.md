---
author: cmohn
comments: true
date: 2014-04-29 08:18:35+00:00
layout: post
link: http://vninja.net/vmware-2/vmware-vshield-manager-upgrade-password-issues/
slug: vmware-vshield-manager-upgrade-password-issues
title: VMware vShield Manager Upgrade - Password Issues
wordpress_id: 3159
categories:
- VMware
tags:
- Appliance
- Internationalization
- Password
- VMware
- vShield
- vShield Manager
---

While upgrading a vShield Manager 5.1.1 install to 5.1.4 at a client, I ran into an issue with logging in after a completed upgrade. The username and password used to log in, and subsequently upload the upgrade file, was no longer working after the upgrade finished and the vShield Manager appliance had been rebooted.

![vShieldManagerError](http://vninja.net/wordpress/wp-content/uploads/2014/04/vShieldManagerError.png)

It turns out that this was due to using international characters in the password for the _admin_ user, in this case the Norwegian specialities æ, ø or å.

I was "lucky" enough to have taken a snapshot of the 5.1.1 vShield Manager appliance before performing the upgrade. After reverting back to the pre-upgrade snapshot, logging in, changing the password to one not containing international characters,  and then performing the upgrade procedure again the appliance was upgraded to 5.1.4 and logon could be done with out problems.

_Be careful when using international characters in passwords for any of the VMware appliances, as I suspect this might be an issue also for other components, not only the vShield Manager._
