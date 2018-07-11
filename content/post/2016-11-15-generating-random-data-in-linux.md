---
author: cmohn
comments: true
date: 2016-11-15 12:00:50+00:00
layout: post
slug: generating-random-data-in-linux
title: Generating Random Data in Linux
url: /virtualization/generating-random-data-in-linux/
wordpress_id: 4359
categories:
- Virtualization
tags:
- fun
- Lab
- Linux
- Veeam Backup &amp; Replication
- VM
---

I've been fleshing out a proper[ Veeam Backup & Replication](http://veeam.com) Demo lab at work, but doing demos on static VMs isn't all that much fun and doesn't really give us much. Doing scheduled backups of non-changing data is really boring.

So, in order to get some changes done on the file system on a few Linux VMs running in the environment, I came up with the following solution:
<!--more-->

I set up a crontab entry that generates a file with random data in it a couple of times a day, just to make sure that there are some changes made to the VM. The crontab entry looks like this:

[cc lang="bash" escaped="true" width="90%" line_numbers="off"]
0 03,09,13,22 * * * head -c 1G </dev/urandom >/tmp/randomdata
[/cc]

This generates a 1 GB file called _randomdata _in _/tmp_ filled with content from [/dev/urandom](https://en.wikipedia.org/wiki//dev/random) at a couple of different times a day. This ensures that there are at least 1GB of changes for each backup cycle, and gives Veeam Backup & Replication something to work with.

#vDM30in30 progress:
[progressbar_circle percent= 30]
