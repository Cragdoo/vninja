---
author: Christian Mohn
comments: true
date: 2016-10-14 13:03:41+00:00
layout: post
slug: macos-secure-pipes
title: 'macOS: Secure Pipes'
url: /osx/macos-secure-pipes/
wordpress_id: 4253
categories:
- OS X
- Software
tags:
- macOS
- Proxy
- SSH
- Tunnels
---

Since I set up a public jumphost for my homelab/network, I've been looking for an easy way to manage my SSH tunnels. After trying a couple of different managers, I've chosen to use [Secure Pipes](https://www.opoet.com/pyro/index.php).![](/img/Screenshot-2016-10-14-11.07.46-300x243.png)

<!--more-->

This little piece of free software allows you to easily manage SSH tunnels, with features like Remote Forwards, Local Forwards and SOCKS proxies. It runs in the toolbar, and makes connections easily available. It even reconfigures your network settings to use the SOCKS proxy, if allowed to. It also uses my private ssh keys to authenticate to the jumphost, which makes me a happy user.

Free and easy to use, what's not to love?
