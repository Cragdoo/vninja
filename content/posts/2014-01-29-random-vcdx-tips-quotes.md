---
author: cmohn
comments: true
date: 2014-01-29 15:58:10+00:00
layout: post
link: http://vninja.net/virtualization/random-vcdx-tips-quotes/
slug: random-vcdx-tips-quotes
title: Random VCDX Tips and Quotes?
wordpress_id: 3031
categories:
- Virtualization
tags:
- Desktop
- featured
- fun
- GeekTool
- PHP
- Quotes
- Script
- VCDX
---

![vNinja-Labs](http://vninja.net/wordpress/wp-content/uploads/2014/01/vNinja-Labs.png)I needed something to spruce up my desktop environment, and it seems that one of the more popular ways to do that is to display random quotes and such on your desktop. Instead of hooking up to an existing quotes database, I simply made my own.

I have collected a few VCDX related tips and quotes, primarily from the archive [Duncan Epping](http://twitter.com/DuncanYB) put together of [VCDX Tips from VCDX 001 John Arrasjid](http://www.yellow-bricks.com/2009/10/01/vcdx-tips-from-vcdx-001-john-arrasjid/), but also from the "submissions" I got from [VCDX? Give Me a Quote!](http://vninja.net/news/vcdx-give-quote/) **If anyone else wants their quote/tip/hint added to the database, please let me know!**

But gathering all of these in a single location has no real purpose, so I managed to code up a really small PHP script that picks a random quote, and return it with attribution. The output is available at [http://vninja.net/labs/quotes/quote.php](http://vninja.net/labs/quotes/quote.php) and should return a random line from the db on each query.

I use this in combination with [GeekTool](http://projects.tynsoe.org/en/geektool/) to display a random, changing, quote right on my desktop:

[![VCDX Quotes on the Desktop](http://vninja.net/wordpress/wp-content/uploads/2014/01/VCDXQuotes-1024x640.png)](http://vninja.net/wordpress/wp-content/uploads/2014/01/VCDXQuotes.png)



Inside [GeekTool](http://projects.tynsoe.org/en/geektool/), I run the following command to generate the output:
[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
curl http://vninja.net/labs/quotes/quote.php
[/cc]

This  is set to refresh every 120 seconds, as long as I have internet connectivity.

_Feel free to use the script to do something similar, or if you have a better idea for it's usage let me know!_

Seen "In the Wild" Gallery:

[gallery ids="3065"]
