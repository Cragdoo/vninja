---
author: cmohn
comments: true
date: 2013-05-14 11:23:13+00:00
layout: post
slug: quick-and-dirty-http-based-deployment
title: Quick and Dirty HTTP Based Deployment
url: /virtualization/quick-and-dirty-http-based-deployment/
wordpress_id: 2577
categories:
- Virtualization
tags:
- automation
- Deployment
- fun
- http
- Python
- realworld
- vMA
- VMware
- vSphere
---

A lot of the scripted installation tools that VMware offers allows the usage of a central HTTP based repository for hosting the files. Today I stumbled over a little gem that might just help you create a "quick and dirty" HTTP based deployment scenario by running a simple command in your terminal. By default, this command works on any system that has [Python](http://www.python.org) installed on it, so OS X and Linux should be ready to go as is. As for you Windows users out there, well, your mileage will vary.

The trick here is a one line Python command that simply creates a HTTP server listing the files in your current directory over a given port. On my MacBook, I opened Terminal and ran the following command:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
python -m SimpleHTTPServer 8000
Serving HTTP on 0.0.0.0 port 8000 ..

[/cc]

If I then open my browser, and point it to the IP address of my MacBook, I get a directory listing showing the contents of the current directory.

[![Quick and Dirty HTTP Deployment #01](http://vninja.net/wordpress/wp-content/uploads/2013/05/QnDHTTP01-300x196.png)](http://vninja.net/wordpress/wp-content/uploads/2013/05/QnDHTTP01.png)



As you can see, the directory contains a few files, namely a [Damn Small Linux](http://virtuallymikebrown.com/tag/damn-small-linux-ovf/) appliance packaged as an OVA, as well as the Linux installation files for [ovftool](http://www.vmware.com/resources/techresources/1013).

In this particular case, I wanted to install ovftool inside a running [vMA](http://www.vmware.com/support/developer/vima/) instance I had in my infrastructure. So, by running the following commands I got ovftool downloaded via HTTP, from my MacBook, inside a running vMA instance by only downloading the files in to a given directory and serving it via HTTP from there to vMA.

By running the following command (output edited for verbosity)

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
vi-admin@record:> wget http://192.168.5.62:8000/VMware-ovftool-3.0.1-801290-lin.x86_64.bundle && sudo sh VMware-ovftool-3.0.1-801290-lin.x86_64.bundle
100%[======================================>] 36,631,447  1.46M/s   in 23s
2013-05-14 12:13:06 (1.52 MB/s) - `VMware-ovftool-3.0.1-801290-lin.x86_64.bundle.saved [36631447/36631447]
...
vi-admin's password:
Extracting VMware Installer...done.
...
Do you agree? [yes/no]: yes
...
Installing VMware OVF Tool component for Linux 3.0.1
Configuring...
[######################################################################] 100%
Installation was successful.
vi-admin@record:/tmp>
[/cc]_

I was able to download and install ovftool, in one command.

_The same deployment method could also easily be used to install host patches, deploy OVF based appliances and even install VIB files from a central repository. In fact, that`s the next thing to do here, deploy the Damn Small Linux appliance, by using the newly installed ovftool package.
The flexibility of having a small HTTP server available by running a single command is great, and I`m sure there are many other use cases that I have yet to consider._
