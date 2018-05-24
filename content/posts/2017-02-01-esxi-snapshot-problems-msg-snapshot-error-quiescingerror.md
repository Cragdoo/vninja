---
author: cmohn
comments: true
date: 2017-02-01 13:18:04+00:00
layout: post
slug: esxi-snapshot-problems-msg-snapshot-error-quiescingerror
title: 'ESXi Snapshot Problems: msg.snapshot.error-QUIESCINGERROR'
url: /vmware-2/esxi-snapshot-problems-msg-snapshot-error-quiescingerror/
wordpress_id: 4558
categories:
- VMware
tags:
- ESXi
- NTP
- Snapshot
- vSphere
---

[caption id="attachment_4559" align="alignright" width="300"][![](http://vninja.net/wordpress/wp-content/uploads/2017/02/eikbsc3sdti-sonja-langford-300x200.jpg)](https://unsplash.com/@sonjalangford) Photo by Sonja Langford[/caption]

Just a quick post about something I experienced at a client, with _ESXi 6.0 hosts_, today:

If you have trouble performing VMware snapshots, and see a  _msg.snapshot.error-QUIESCINGERROR_ error, **check the host time settings and NTP**.

In this case, snapshots of VMs located on other hosts in the cluster were fine, but once a VM was moved to the new host, snapshot operations failed after an hour or so.

It turns out a new host in the cluster was not properly set up to use NTP, and time drift between the host and the vCenter caused the snapshot failures. Correcting the time on the host and configuring NTP resolved the issue.

Always remember: **If the problem isn't DNS, it almost certainly is NTP.**
