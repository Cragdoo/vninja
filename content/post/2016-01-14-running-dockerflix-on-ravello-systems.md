---
author: cmohn
comments: true
date: 2016-01-14 12:32:59+00:00
layout: post
slug: running-dockerflix-on-ravello-systems
title: Running Dockerflix on Ravello Systems
url: /virtualization/running-dockerflix-on-ravello-systems/
wordpress_id: 3970
categories:
- Virtualization
tags:
- Docker
- Dockerflix
- Hipster IT
- Ravello
- VMware Photon
---

[![Ravello 2016-01-14 13-30-40](/img/Ravello-2016-01-14-13-30-40-300x141.png)Dockerflix](https://github.com/trick77/dockerflix) is a nice little project that allows you to route your Netflix (and other various streaming services) through a SNI Proxy to access content otherwise geo-blocked. Of course, this requires that you have a VM with for instance an US IP to provide the breakout network, and that's where Ravello Systems comes into the equation. Luckily as a current vExpert I have access to [1000 free monthly CPU hours](http://vninja.net/virtualization/ravello/) of personal/lab usage, all with a [choice of regions](https://support.ravellosystems.com/hc/en-us/articles/215793017) to put the VM in. _Perfect._

<!--more-->


[![](/img/unnamed.png)](http://go.ravellosystems.com/d80sH0DKRI0UC00C56040C0)I created a [VMware Photon](https://vmware.github.io/photon/) based VM on Ravello, with an [Elastic IP](https://www.ravellosystems.com/blog/providing-external-access-application-elastic-ip-addressing/) that allows the IP to stick to the VM, even if it's moved to another public cloud,  and installed Dockerflix. VMware Photon doesn't come with _docker-compose_, which Dockerflix is dependant on, check [Install Docker Compose](https://docs.docker.com/compose/install/) for details on how to install it. Once that is installed, run the following command to download and install Dockerflix.

{{< highlight bash >}}
git clone https://github.com/trick77/dockerflix.git[/cc]
{{< /highlight >}}

Setup of Dockerflix itself is pretty straight forward, just follow the [README](https://github.com/trick77/dockerflix/blob/master/README.md) provided by the project. Make sure you enable http/https/ssh to the VM with a public IP. Once that was done, I set up [dnsmasq](http://www.thekelleys.org.uk/dnsmasq/doc.html) on one of the Linux VMs in my home-lab, with the output config the python script provided by Dockerflix.

{{< highlight bash >}}
python ./gendns-conf.py --remoteip [RAVELLO_PUBLIC_IP]
{{< /highlight >}}


**Note:** _Running this permanently would be a violation of the Ravello vExpert Terms of Service. This is to be considered a technical Proof of Concept, more than a permanent setup. I haven't investigated the possibility to set up something like this permanently on either vCloud Air, AWS, Azure or Google Cloud Platform, or if you are even allowed to do so. But then again, all you are doing is routing IP traffic..._

As far as testing goes, I experienced no problems with bandwidth or anything else. Streaming was perfect, and seamless. The fact that I was routing the traffic through a VM running in the US was not noticeable at all.

Configuring it was a great exercise. Not only did I get to play with VMware Photon and Docker, but it also shows that using Ravello Systems to perform Proof of Concept scenarios is perfectly viable, even enjoyable.
