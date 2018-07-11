---
author: cmohn
comments: true
date: 2017-03-28 11:44:58+00:00
layout: post
slug: cross-vcenter-vm-mobility-fling-macos
title: Cross vCenter VM Mobility Fling - macOS?
url: /vmware-2/cross-vcenter-vm-mobility-fling-macos/
wordpress_id: 4612
categories:
- VMware
tags:
- Awesome
- Fling
- macOS
- VMware
- xvMotion
---

The [VMware Cross vCenter VM Mobility - CLI](https://labs.vmware.com/flings/xvc-mobility-cli#requirements) was recently updated so I decided to try it out. In short, this little Java based application allows you to easily move or clone VMs between disparate vCenter environments.

<!--more-->


The Fling is listed with the following requirements:


  * JDK 1.7 or above


  * Two vCenter instances with ESX 6.0


  * Windows : Windows Server 2003 or above


  * Linux : RHEL 7.x or above, Ubuntu 11.04 or above



There is no mention of macOS there, but I decided to give it a go any way, and i**t turns out that it works just fine on macOS as well!
**Just make sure you have the Java JDK installed locally. When I ran it the first time, I got the following error, since the _JAVA_HOME_ environment variable was not set.

[cc lang="bash"]
~/Downloads/xvc-mobility-cli_1.2$ sh xvc-mobility.sh
set JAVA_HOME to continue the operation
[/cc]

This is very easy to fix, just run the following command in your terminal of choice, and _xvc-mobility.sh_ should work just fine on your Mac.

[cc lang="bash"]
export JAVA_HOME=$(/usr/libexec/java_home)
[/cc]

Next up is running the fling with the correct parameters (this is a clone operation, not a relocate):

[cc lang="bash"]
~/Downloads/xvc-mobility-cli_1.2$ sh xvc-mobility.sh -svc [source-vcenter] -su [source-vcenter-username]
-dvc [destination-vcenter] -du [destination-vcenter-username]
-vms [vm-name] -dh [destination-host]
-dds [destination-datastore] -op clone -cln [destination-vm-name]
...
13:41:40.591 [main] INFO com.vmware.sdkclient.vim.Task - CloneVM_Task | State = SUCCESS | Error = null | Result = com.vmware.vc.ManagedObjectReference@d9a221a7
13:41:40.597 [main] INFO com.vmware.sdkclient.vim.Task - Monitor task end
13:41:40.597 [main] INFO com.vmware.sdkclient.vim.Task - CloneVM_Task took : 0:51:33.728
13:41:40.603 [main] INFO c.v.s.helpers.CrossVcProvHelper - Successfully cloned the vm:[destination-vm-name]
[/cc]

I was able to clone a VM from my lab in Bergen to my lab in Oslo, without any problems what-so-ever. Not only is that a Cross vCenter vMotion, but also a Cross Country one, awesome!

Now this is just an example, please check the [official documentation](https://download3.vmware.com/software/vmw-tools/xvc-mobility-cli/instructions_new.pdf) for all the parameters, and what the tool expects.
