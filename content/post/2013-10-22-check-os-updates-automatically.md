---
author: Christian Mohn
comments: true
date: 2013-10-22 18:42:42+00:00
layout: post
slug: check-os-updates-automatically
title: Check for OS X Updates Automatically
url: /osx/check-os-updates-automatically/
wordpress_id: 2823
categories:
- OS X
tags:
- Apple
- automation
- Impatient
- Mac
- OS X
---

Yeah, I admit it. I want OS X Mavericks, and I want it now.

Unfortunately, it´s not available yet from Software Update. So instead of manually checking every 5 minutes or so, I decided to create a small bash script that does it for me.

It´s very, very simple, but I think it does the job:

First off, pop into Terminal and get root access:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
h0bbel::h0bair { ~ }-> sudo su -
[/cc]

Then create a small bash script, I named mine _update.sh_, that contains the following:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
while true
do
softwareupdate -l
sleep 60
done
[/cc]

Change it to be executable by running
[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
chmod +x update.sh
[/cc]

Then run the script, to have softwareupdate run over and over again (60 seconds after it completes) until you break it with _ctrl-c_

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
h0bair:~ root# ./update.sh
Software Update Tool
Copyright 2002-2010 Apple

No new software available.
[/cc]

At least I now get warning once it´s available. **I can has Mavericks nao?**
