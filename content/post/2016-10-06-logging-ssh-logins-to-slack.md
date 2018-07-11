---
author: cmohn
comments: true
date: 2016-10-06 11:31:51+00:00
layout: post
slug: logging-ssh-logins-to-slack
title: Logging SSH logins to Slack
url: /homelab/logging-ssh-logins-to-slack/
wordpress_id: 4224
categories:
- Homelab
tags:
- Awesome
- Homelab
- HomeLabOps
- Slack
---

I'm using [Slack to alert and log ](http://vninja.net/homelab/homelabops-via-slack/)a few things in my environment, and one of the things I use it for is to alert me if someone logs on via SSH to my public facing Jumphost.

_For a good walkthrough on how to set up such a host, check out [Tunnel all your remote connections through ssh with a linux jumpbox](http://www.virtualtothecore.com/en/tunnel-all-your-remote-connections-through-ssh-with-a-linux-jumpbox/) by [Luca Dell'Oca](https://twitter.com/dellock6)._

<!--more-->

My Ubuntu 16.04 Jumphost is set up to only accept Key-Based Authentication, to secure it as much as possible, but I would still like to get instant notification if someone logs into it interactively.



## How to set up SSH login notification to Slack.


  1. ![screenshot-2016-10-06-13-04-51](/img/Screenshot-2016-10-06-13.04.51-300x182.png)First of all, we need  an Incoming WebHook in Slack in order to receive the notifications.
You configure those from the **Apps & Integration **menu item. This in turn opens up the Slack App Directory, find **Build** on the top right and then choose **Make a Custom Integration**.


  2. ![screenshot-2016-10-06-13-08-09](/img/Screenshot-2016-10-06-13.08.09-300x60.png)One your are in the **Build a Custom Integration** section, find (or search) **Incoming WebHooks** and select that.


  3. Next up, define which Slack channel should be the integration point, and click on **Add Incoming WebHooks** integration.


  4. Copy the Webhook URL presented on the next screen
**Note:** keep this one a secret, anyone with access to this URL will be able to post to your Slack channel.


  5. On my Ubuntu 16.04 Linux jumphost I've created a small bash script called _/etc/ssh/notify.sh_. This script utilizes _curl _ and the WebHook URL to post information directly to Slack. The script looks like this:

  **notify.sh**

{{< highlight bash >}}
#!/bin/sh
url="https://hooks.slack.com/services/*********"
channel="#messages"
host="`hostname`"
content="\"attachments\": [ { \"mrkdwn_in\": [\"text\", \"fallback\"], \"fallback\": \"SSH login: $USER connected to \`$host\`\", \"text\": \"SSH login to \`$host\`\", \"fields\": [ { \"title\": \"User\", \"value\": \"$USER\", \"short\": true }, { \"title\": \"IP Address\", \"value\": \"$SSH_CLIENT\", \"short\": true } ], \"color\": \"#F35A00\" } ]"
curl -s -X POST --data-urlencode "payload={\"channel\": \"$channel\", \"mrkdwn\": true, \"username\": \"ssh-bot\", $content, \"icon_emoji\": \":computer:\"}" $url
/bin/bash
{{< / highlight >}}



Replace the  the WebHook URL with your own from step 4 and which channel to post to and you should be ready to go.  This script logs the username and the IP address the connection comes from, and then posts it to the Slack WebHook with the help of _curl_.

**Note:** I've chosen to include the WebHook name etc in the script itself, instead of via the WebHook definition on Slack, mostly since I don't want to create a WebHook for all hosts I want logging from. With this setup, I can just change the username part of the _curl _command. It already logs the hostname, so this is pretty much superficial, but hey, that's how I made it.


  6. _chmod +x /etc/ssh/notify.sh_ to make it executable, and test it. If everything works as expected, you should see an immediate log entry in your chosen Slack channel.


  7. On order to make this script runs every time someone logs into the Jumphost, I added a ForceCommand to the end of my_ /etc/ssh/sshd_config _file, like this:[cc lang="bash" escaped="true"]
ForceCommand /etc/ssh/notify.sh
[/cc]



And that's it. A login via SSH on the Jumphost now looks like this in my Slack channel:

![screenshot-2016-10-06-13-26-10](http://vninja.net/wordpress/wp-content/uploads/2016/10/Screenshot-2016-10-06-13.26.10-1-1024x179.png)

How awesome is that? Of course, this just scratches the surface of what is possible with Slack's Incoming WebHooks, I'm using a similar approach for logging new devices discovered in phpmyipam but that's for another post.
