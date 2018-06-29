---
author: cmohn
comments: true
date: 2013-07-31 23:10:42+00:00
layout: post
slug: slate-setup
title: My Slate Setup
url: /osx/slate-setup/
wordpress_id: 2623
categories:
- OS X
tags:
- KeyRemap4MacBook. Customization
- OS X
- PCKeyboardHack
- Setup
- Slate
- Software
- Window Manager
---

![macbook-pro-too-mainstream-1.jpg](http://vninja.net/wordpress/wp-content/uploads/2013/08/macbook-pro-too-mainstream-1.jpg-199x300.png)About a year and a half I go, I took the leap from running Microsoft Windows as my main operating system and switched into full "hipster mode", i.e. switched to a Macbook Air and OS X.

Simply put, "the change" was not that hard and most everything has worked without problems, and for those things that still require Microsoft Windows, well, there is [VMware Fusion](http://www.vmware.com/products/fusion/overview.html) for that.

While I´m admittedly still a novice OS X user, and not even close to mastering OS X, I´d like to share my current Slate setup.

[Slate](https://github.com/jigish/slate) is a OS X window manager that makes it much easier to resize, focus and arrange your applications. The real beauty of it is that everything is controlled via the keyboard, no need to reach for the mouse.

By combining [PCKeyboardHack](https://pqrs.org/macosx/keyremap4macbook/pckeyboardhack.html.en) and [KeyRemap4MacBook](https://pqrs.org/macosx/keyremap4macbook/) with [Slate](Slate), I am now able to move my windows to pre-arranged locations on my display, or even "throw" a window to another monitor if required.

None of this was originally my idea, I´ve stolen quite a bit from [Using Slate: A Hacker's Window Manager for Macs](http://thume.ca/howto/2012/11/19/using-slate/) and other sources, but the end result is simply a dream to work with.

As suggested in [A useful Caps Lock key](http://brettterpstra.com/2012/12/08/a-useful-caps-lock-key/), I have remapped my Caps Lock key to F19, and set F19 up as Hyper key (Control, Command, Option and Shift pressed simultaneously), and use that as a trigger in Slate. One thing I always forget to do though, is to disable the Caps Lock key whenever I connect a new keyboard to the Mac. After a recent re-install I could not for the life of me figure out why this setup was not working, until I remembered that I had to disable the key (as mentioned in the article linked to above).



<blockquote>The first thing you’ll need to do is disable the Caps Lock key in OS X. Head to System Preferences’ Keyboard pane and click the “Modifier Keys…” button. Set Caps Lock to “No Action.”</blockquote>



My current .slate config files looks like this



Seasoned Slate users, will notice that I am barely scratching the surface of what is possible here, but this is a work in progress. I really want to set up pre-defined application locations and window sizes based on my various multi-monitor setups (Macbook only, home office, work office, etc.), but that is still a work in progress.

How fun is it being able to hit _caps-lock right-arrow_ and throw an application window from the internal Macbook screen to a secondary screen on my right? And at the same time resize it to fill the entire screen!

I may just be me, but I do find pleasure in these little things. Especially when they just work. Now I just have to remember to use them frequently, muscle-memory is a powerful tool when it comes to efficiency.


