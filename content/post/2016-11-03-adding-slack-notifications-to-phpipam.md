---
author: Christian Mohn
comments: true
date: 2016-11-03 21:41:07+00:00
layout: post
slug: adding-slack-notifications-to-phpipam
title: Adding Slack Notifications to phpipam
url: /virtualization/adding-slack-notifications-to-phpipam/
wordpress_id: 4294
categories:
- Virtualization
---

[As mentioned before](http://vninja.net/homelab/homelabops-via-slack/), I've kinda turned my home lab into some sort of Slack-Ops deal, where various services in my home lab notify me of events in a private Slack channel.

The latest rendition of that, is adding Slack notifications from [phpipam](http://phpipam.net). Once phpipam detects a new device picking up an IP in my network, it notifies me like this:

![screenshot-2016-11-03-22-24-18](/img/Screenshot-2016-11-03-22.24.18-1024x65.png)

In order to get this working, I had to edit the _/var/www/phpipam/functions/scripts/discoveryCheck.php_ file in phpipam. _discoveryCheck.php_ is the script that is run when phpipam does local subnet discovery  check, so this was a natural place to add my custom curl command.

I added the following code on _line 254_ in that file, just below the  code block that ends with
_$Addresses->modify_address($values);_

{{< highlight bash >}}
// Log to Slack -- Run custom CURL script
$command = "curl -s -X POST -H \"Content-type: application/json\" --data '{\"text\":\"$values[lastSeen] - new host autodiscovered: IP: $ip Hostname: $values[dns_name] \"} https://[WEBHOOK URL]";
system($command);
{{< /highlight >}}

Replace [WEBHOOK URL] with your actual URL. For more details on how to set up a webhook URL, check [Logging SSH logins to Slack](http://vninja.net/homelab/logging-ssh-logins-to-slack/).

Pretty easy to do locally, but it would be nice of the phpipam developers could make alerting to third party services a native feature - I'm sure there are other use cases for this as well.
