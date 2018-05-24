---
author: cmohn
comments: true
date: 2014-12-12 12:24:55+00:00
layout: post
slug: importing-vcloud-air-ssl-certificate-vcenter-server-appliance-5-x
title: Importing vCloud Air SSL Certificate on the vCenter Server Appliance 5.x
url: /vmware-2/importing-vcloud-air-ssl-certificate-vcenter-server-appliance-5-x/
wordpress_id: 3470
categories:
- VMware
tags:
- Certificate
- SSL
- vCenter
- vCenter Web Client
- vCloud Air
- VCSA
---

I'm playing around a bit with vCloud Air and Virtual Private Cloud OnDemand, and in order to set up the vCloud Hybrid Service plugin in the vSphere Web Client you need to import the vCloud Air SSL certificate into vCenter. If the certificate isn't present in the vCSA keystore when you try to authenticate with vCloud Air, you get a "**Server Certificate not Verified**" error, and you will be unsuccessful in configuring the plugin.

The [Using the vCloud Hybrid Service vSphere Client Plug-in document](http://pubs.vmware.com/vchsplugin-10/topic/com.vmware.ICbase/PDF/vchs_plugin_using.pdf) outlines how this can be accomplished, but it's based on downloading the SSL certificate via a browser and then importing it into the vCenter Keystore. Since I mostly run the vCenter Server Appliance, I didn't want to bother with downloading it from one of my desktops, and copying the files over to the vCSA for import.

I mean, there has to be a better way to do that, via the command line, right? Indeed there is, this little one-liner downloads and formats the certificate from [vchs.vmware.com](http://vchs.vmware.com) to /tmp on the vCSA, and then proceeds to import it into the keystore.



[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
echo -n | openssl s_client -connect vchs.vmware.com:443 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /tmp/vchs.cer && /usr/java/jre-vmware/bin/keytool -alias vchs -v -keystore /usr/lib/vmware-vsphere-client/server/configuration/keystore -storepass changeit -import -file /tmp/vchs.cer
[/cc]

All you have to to is press 'y' to confirm the import:
[cc lang="bash" width="100%" theme="blackboard" nowrap="1"]
Trust this certificate? [no]: y
Certificate was added to keystore
[Storing /usr/lib/vmware-vsphere-client/server/configuration/keystore]
vcenter:/tmp #
[/cc]

And there it is, you can now add your vCloud Air credentials via the vSphere Web Client, without having to copy any files from your browser/desktop to the vCSA.
