---
author: cmohn
comments: true
date: 2016-11-02 00:19:30+00:00
layout: post
slug: running-telnetlogger-on-my-home-ip
title: Running telnetlogger on my home IP
url: /virtualization/running-telnetlogger-on-my-home-ip/
wordpress_id: 4284
categories:
- Virtualization
---

[Robert Graham](https://twitter.com/ErrataRob) of [erratasec](http://blog.erratasec.com) has created a small honeypot tool called [telnetlogger](https://github.com/robertdavidgraham/telnetlogger).



<blockquote>
  This is a simple program to log login attempts on Telnet (port 23).
  It's designed to track the Mirai botnet. Right now (Oct 23, 2016) infected Mirai machines from around the world are trying to connect to Telnet on every IP address about once per minute. This program logs both which IP addresses are doing the attempts, and which passwords they are using.
</blockquote>



For those still unaware of what the Mirai botnet is, it's basically malware that scans for vulnerable devices with port 23 (telnet) open to the outside world, and tries to log on with known hardcoded credentials.



<!--more-->Compromised devices have then been used to launch some of the largest DDoS attacks seen to date. For more details, check out [Breaking Down Mirai: An IoT DDoS Botnet Analysis](https://www.incapsula.com/blog/malware-analysis-mirai-ddos-botnet.html) and [Double-dip Internet-of-Things botnet attack felt across the Internet](http://arstechnica.com/security/2016/10/double-dip-internet-of-things-botnet-attack-felt-across-the-internet/)

![Photo credit: gratisography.com](http://vninja.net/wordpress/wp-content/uploads/2016/11/330H-1024x683.jpg) Photo credit: [gratisography.com](http://gratisography.com)

Yes, Mirai is **not** your grandmothers botnet.

I figured it would be a nice little thing to try out, so I spun up a small Linux VM, compiled telnetlogger [telnetlogger](https://github.com/robertdavidgraham/telnetlogger), ran it and opened inbound port 23 (telnet) on my firewall at home.

And guess what, it took all of 1 second before I saw the first connection attempt! I let the honeypot service run for a few hours (about 8 or so), and here are the results, as aggregated by [HoneyCredIPTracker](https://github.com/danielmiessler/HoneyCredIPTracker/blob/master/HoneyCredIPTracker.sh) by [Daniel Miessler](https://twitter.com/danielmiessler/status/793138522597187584)



#### Top connection attempts, sorted by country

36 TW
32 VN
28 BR
26 TR
22 IN
18 RU
14 UA
12 US
12 CN
8 PK
8 MX
8 FR
6 TT
6 PL
6 KR
4 TH
4 SE
4 RO
4 PY
4 MG
4 KH
4 IR
4 GB
4 CR
4 CA



#### Top Attempted Credentials



415 root xc3511
410 root vizxv
385 root admin
255 admin password
250 admin admin
240 root root
235 root 888888
215 root 123456
175 root default
170 root juantech
170 root 54321
165 support support
155 root xmhdipc
130 admin admin1234
125 guest guest
120 root Zte521
120 root 12345
115 root klv123
100 admin smcadmin
95 root anko
90 root GM8182
90 root 1234
90 root 1111
80 root pass
75 guest 12345

**In those 7 hours this was running, I saw a total of 15785 connection attempts, a connection attempt every 1.8 seconds - on average.**

I guess it's best to close port 23 again, for good this time.

Hat tip to a former colleague of mine, security afficionado and all around great guy [Per Thorsheim](https://twitter.com/thorsheim) for letting me know about this [tool](https://twitter.com/thorsheim/status/793055315369549824).
