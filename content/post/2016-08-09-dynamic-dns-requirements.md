---
author: cmohn
comments: true
date: 2016-08-09 22:18:57+00:00
layout: post
slug: dynamic-dns-requirements
title: Dynamic DNS Requirements
url: /homelab/dynamic-dns-requirements/
wordpress_id: 4143
categories:
- Homelab
tags:
- Cloudflare
- DNS
- Dynamic DNS
- Homelab
- Scripting
---

While working on my new [Homelab](http://vninja.net/news/taking-it-to-far/) setup, I've been investigating ways to provide hostname based access to several web services located in my DMZ zone. Since my provider doesn't provide static IP addresses, I also need an external Dynamic DNS service, to provide said hostname mappings through the reverse proxy on the inside.

There are loads of Dynamic DNS services available, most of them lets you use some sort of predefined domain name scheme, and point it to your external IP, but I wanted to use a domain name that I own and control. Since I use [CloudFlare](https://cloudflare.com) to provide DNS services (amongst other things) for this very site, it was a natural choice to see if they could fit the bill for my lab needs as well. Turns out, not only can they provide the services I need for free, they also allow me to play around and have fun at the same time!

<!--more-->


##### My setup looks like this:


![Logical Web Services Access Diagram](/img/conceptualOutsideAcess2.png) Logical Web Services Access Diagram


As seen in the diagram, the setup is pretty simple. As far as Dynamic DNS requirements go, all I need to be able to do (for now) is to update the IP address for a couple of [A records](https://support.dnsimple.com/articles/a-record/) for my domain, if my external IP changes.  This in turn makes the reverse proxy work, since it redirects traffic based on hostname when there is an incoming request.

CloudFlare offers several methods to update the records, including [ddclient](https://www.cloudflare.com/resources-downloads/), but that requires Perl, and frankly that's no fun at all. What it fun however, is to update A records directly through the [CloudFlare API](https://api.cloudflare.com), so that's the route I headed down.

I moved the domain name I want to use for external access to my lab, to CloudFlare and rolled my own small Dynamic DNS Updater all within an hour or so of actual work.

Not bad at all, especially considering I didn't even know they provided the possibility to do so, or that they provide a public API you can play with.

In the next post in this series, I'll show how [I solved it with some simple API calls and some bash scripting.](http://vninja.net/homelab/using-cloudflare-for-dynamic-dns/)
