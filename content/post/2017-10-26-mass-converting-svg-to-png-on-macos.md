---
author: cmohn
comments: true
date: 2017-10-26 13:32:01+00:00
layout: post
slug: mass-converting-svg-to-png-on-macos
title: Mass Converting .svg to .png on macOS
url: /osx/mass-converting-svg-to-png-on-macos/
wordpress_id: 4813
categories:
- OS X
tags:
- Blogtober
- HomeBrew
- macOS
---

When playing around with [Royal TSX](http://vninja.net/osx/making-royal-tsx-even-more-awesome/) I needed to mass convert the [VMware Clarity](https://github.com/vmware/clarity/tree/master/src/clarity-icons) .svg files to .png files that I could use as icons in Royal TSX.

After trying a series of different approaches, I ended up with using [rsvg-convert from libRSVG](https://wiki.gnome.org/action/show/Projects/LibRsvg?action=show&redirect=LibRsvg). In order to get _rsvg-convert_ installed on my MacBook, I turned to [HomeBrew](https://brew.sh).

HomeBrew, which calls itself **The missing package manager for macOS **is in my opinion essential for any macOS user. If you are missing a command or utility, chances are that HomeBrew has you covered.



### Mass Converting:



Once you have HomeBrew installed, you're pretty much ready to go by running the following command in Terminal:


    
    brew install librsvg



This installs the [libRSVG](http://formulae.brew.sh/formula/librsvg) formulae, and all it's dependencies, and makes rsvg-convert available.

Once libRSVG installed locally, you can mass-convert .svg files by running the following command in your terminal of choice.


    
    for i in *; do rsvg-convert $i -o `echo $i | sed -e 's/svg$/png/'`; done



This loops through every .svg file in the current directory, and creates .png versions of them, for usage elsewhere.
