---
author: Christian Mohn
comments: true
date: 2015-08-26 16:45:10+00:00
layout: post
slug: update-esxi-embedded-host-client-fling
title: Update ESXi Embedded Host Client Fling
url: /vmware-2/update-esxi-embedded-host-client-fling/
wordpress_id: 3766
categories:
- VMware
tags:
- Embedded Host Client
- ESXi
- vSphere
---

![VMware ESXi Host Client 2015-08-26 18-42-24](/img//VMware-ESXi-Host-Client-2015-08-26-18-42-24-300x164.png)The ESXi Embedded Host Client Fling [got an upgrade today](http://www.virtuallyghetto.com/2015/08/esxi-embedded-host-client-fling-updated-to-v2.html), and in addition to new features it now works properly on ESXi 5.5. In addition to this, it's also available as an [offline bundle](http://download3.vmware.com/software/vmw-tools/esxui/esxui-offline-bundle-3015331.zip) so you can distribute it with Update Manager.

Since I've spent most of my day in esxcli, here is a quick post on how to perform the upgrade from a [local http repository](http://vninja.net/vmware-2/esxi5-5-to-6-0-upgrade-from-local-http-daemon/) hosting the .vib file.

<!--more-->


### Install .vib file


  1. Download the [esxui-3015331.vib](https://labs.vmware.com/flings/esxi-embedded-host-client) file and place it somewhere accessible via http.


  2. SSH to your ESXi host, and run the following command (remember to enable maintenance mode if needed). {{< highlight bash >}}
esxcli software vib install -v http://[yourip:port/path]/esxui-3015331.vib{{< /highlight >}}


  3. Wait for the installation to finish{{< highlight bash >}}
Installation Result
Message: Operation finished successfully.
Reboot Required: false
VIBs Installed: VMware_bootbank_esx-ui_0.0.2-0.1.3015331
VIBs Removed: VMware_bootbank_esx-ui_0.0.2-0.1.2976804
VIBs Skipped:
{{< /highlight >}}


  4. Access the updated Embedded Host Client via http://hostip/ui/
