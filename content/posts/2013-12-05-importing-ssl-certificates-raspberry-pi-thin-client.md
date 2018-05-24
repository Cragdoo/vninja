---
author: cmohn
comments: true
date: 2013-12-05 22:47:32+00:00
layout: post
slug: importing-ssl-certificates-raspberry-pi-thin-client
title: Importing SSL Certificates to Raspberry Pi Thin Client
url: /virtualization/importing-ssl-certificates-raspberry-pi-thin-client/
wordpress_id: 2916
categories:
- Virtualization
tags:
- Certificates
- Citrix
- Raspberry Pi
- Receiver
- SSL
- Thin Client
---

When playing around with the [Raspberry Pi Thin Client](http://rpitc.blogspot.com), I ran into an issue with the SSL certificates for the Citrix Receiver client. For some reason it didn't want to play with the certificates installed on the server side, and popped the following error message:


<blockquote>**You have not chosen to trust "AddTrust External CA Root", the issuer of the server's security certificate.**</blockquote>


Thankfully there is a quick fix for this! Since Iceweasel is also in the RPTC distribution, and it has a lot more SSL root CA certificates included by default, all that was required (in my case) was to link the certificates it has with the certificates the Citrix Receiver client can use.

Issue the following command to create symlinks for the "missing" certificates in the Citrix Receiver keystore:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
sudo ln -s /usr/share/ca-certificates/mozilla/* /opt/Citrix/ICAClient/keystore/cacerts/
[/cc]

supply in the root password, which in RPTC is raspberry by default, and if your CA's root certificate is included with Iceweasel, you should now be able to connect without getting certificate errors.

It's pretty neat to be able to use small Raspberry Pi as a Thin Client like this, but it's too bad that it does not support VMware Horizon View using PCoIP (yet?), only RDP, something I have yet to test since my demo environment runs PCoIP only at the moment.

Thanks again to [Simplivity](http://www.simplivity.com) for their [Raspberry Pi vEXPERT](http://simplivity.sites.hubspot.com/register-for-your-vexpert-raspberry-pi) gift!
