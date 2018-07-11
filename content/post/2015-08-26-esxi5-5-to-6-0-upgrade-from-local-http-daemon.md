---
author: cmohn
comments: true
date: 2015-08-26 11:35:08+00:00
layout: post
slug: esxi5-5-to-6-0-upgrade-from-local-http-daemon
title: ESXi5.5 to 6.0 Upgrade From Local HTTP Daemon
url: /vmware-2/esxi5-5-to-6-0-upgrade-from-local-http-daemon/
wordpress_id: 3750
categories:
- VMware
tags:
- ESXi
- vCenter
- VMware
- vSphere
---

I was recently involved with consulting for a Norwegian shipping company who has quite a few remote vSphere installations, most of them with a couple of ESXi hosts, but no vCenter and hence no Update Manager. While looking at methods for managing these installations, in particular how to facilitate patching and upgrading scenarios, I remembered that way back in 2013, I posted [Quick and Dirty HTTP-based Deployment](http://vninja.net/virtualization/quick-and-dirty-http-based-deployment/) which shows how to use the Python to run a simple http daemon, and serve files from it.
<!--more-->

Surely something similar can be used to maintain a central repository for vSphere patches? While I don't recommend using your MacBook as a permanent source for these updates, you really should set up a proper http server in your network and utilize that, it works as a proof of concept.



##### So here it is, a recipe for using a simple http daemon to host your own ESXi Offline Bundles, and how to upgrade from 5.5 to 6.0 from the command line.

    1. Download the offline bundle you want to use, in my case I used _ESXi600-201507001.zip_ (ESXi 6.0.0b).
    2. Extract the .zip file into it's own directory, served by the http daemon, and rename it for simplicity.
    3. Connect to your target ESXi host via SSH
    4. Check the current running ESXi version by running _esxcli system version{{< highlight bash >}}
The time and date of this login have been sent to the system logs.VMware offers supported, powerful system administration tools. Please
see www.vmware.com/go/sysadmintools for details.The ESXi Shell can be disabled by an administrative user. See the
vSphere Security documentation for more information.
~ # esxcli system version get
Product: VMware ESXi
Version: 5.5.0
Build: Releasebuild-1623387
Update: 1
~ #{{< /highlight >}}
Enable outgoing http requests from the ESXi host by running _esxcli network firewall ruleset set -e true -r httpClient_


    6. Determine which profile to use by running __esxcli software sources profile list -d [http://daemon-ip:port/directory/]
{{< highlight bash >}}
~ # esxcli software sources profile list -d http://172.29.100.248:8000/6.0/
Name Vendor Acceptance Level
-------------------------------- ------------ ----------------
ESXi-6.0.0-20150701001s-standard VMware, Inc. PartnerSupported
ESXi-6.0.0-20150701001s-no-tools VMware, Inc. PartnerSupported
ESXi-6.0.0-20150704001-no-tools VMware, Inc. PartnerSupported
ESXi-6.0.0-20150704001-standard VMware, Inc. PartnerSupported
~ #{{< /highlight >}}


    7. Run _vim-cmd hostsvc/maintenance_mode_enter_ to put ESXi host into maintenance mode.


    8.  Run _esxcli software profile update -d [http://daemon-ip:port/directory/] -p [profilename] _to fetch and install from http daemon{{< highlight bash >}}
# esxcli software profile update -d http://172.29.100.248:8000/6.0/ -p ESXi-6.0.0-20150704001-standard
Update Result
Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
Reboot Required: true
VIBs Installed: [long list of vibs removed for brevity]
VIBs Skipped:
~ #{{< /highlight >}}


    9. Disable outgoing http client traffic by running _esxcli network firewall ruleset set -e false httpClient_


    10. Reboot host by running the _reboot_ command


    11. When host has rebooted, connect to ESXi host again via SSH and run _esxcli system version get
{{< highlight bash >}}[root@localhost:~] esxcli system version get
Product: VMware ESXi
Version: 6.0.0
Build: Releasebuild-2809209
Update: 0
Patch: 11
[root@localhost:~]{{< /highlight >}}
_


    12. Verify that it runs the correct version, and run _vim-cmd hostsvc/maintenance_mode_exit_ to put the host out of maintenance mode.




And that's it, the host has now been upgraded from ESXi 5.5 to 6.0, from a local centralized http-based repository without the need to connect to the outside world. All done via the command line, and without a vCenter with Update Manager. Pretty neat.
