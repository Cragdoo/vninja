---
author: cmohn
comments: true
date: 2017-10-11 21:45:11+00:00
layout: post
link: http://vninja.net/osx/making-royal-tsx-even-more-awesome/
slug: making-royal-tsx-even-more-awesome
title: Making Royal TSX Even More Awesome
wordpress_id: 4751
categories:
- OS X
- Software
tags:
- Awesome
- Blogtober
- management
- RoyalTSX
- Software
---

For those who don't know, [Royal TSX](https://www.royalapplications.com/ts/mac/features) is an awesome Remote Management solution, which supports RDP, VNC, SSH, S/FTP and even ESXi and vCenter. I've been using it for years, not just because they offer [free licenses for vExperts](https://www.royalapplications.com/ts/nfr/) (and others), but simply because it works really well. Store it's config file on a synchronized file area (like Dropbox), and boom, your config follows you around from machine to machine, including custom icons. What's _not_ to like?

Following [Ryan Johnson's tweet](https://twitter.com/tenthirtyam/status/913734554216693765), where he showed off his [VMware Clarity](https://github.com/vmware/clarity/tree/master/src/clarity-icons) inspired [Royal TSX](https://www.royalapplications.com/ts/mac/features) setup, I decided to do something similar. Unlike Ryan, I decided to run with the standard Clarity icons, and not invert them. Since the Clarity icons are in .svg format, I had to convert them to .png to be able to use them as icons in Royal TSX, [I'll post a separate post on how I batch converted them later](http://vninja.net/osx/mass-converting-svg-to-png-on-macos/).



### Currently, my setup looks like this



[caption id="attachment_4760" align="alignnone" width="186"][![](http://vninja.net/wordpress/wp-content/uploads/2017/10/Screenshot-2017-10-11-23.16.25-186x300.png)](http://vninja.net/wordpress/wp-content/uploads/2017/10/Screenshot-2017-10-11-23.16.25.png) Royal TSX with Clarity icons[/caption]

Changing the icons for entries is pretty straight forward. For existing entries in your config file, simply open the items **properties** and click on the small icon besides the Display Name. This brings up a dialog showing the built-in icons, but also reveals an option to browse your filesystem for a new icon to use.

[![](http://vninja.net/wordpress/wp-content/uploads/2017/10/Screenshot-2017-10-11-23.21.48-644x305.png)](http://vninja.net/wordpress/wp-content/uploads/2017/10/Screenshot-2017-10-11-23.21.48.png)

**Update:** Felix from Royal Applications left a [nice comment](http://vninja.net/osx/making-royal-tsx-even-more-awesome/#comment-19281), explaining that you can also drag-and-drop icons directory from finder into Royal TSX as well as the manual process described above.

To change the default icons, find **Default Settings** in the Navigation Panel on the left, and follow the same procedure.

While the primary goal was to prettify my setup with snazzy new icons, I discovered that I could do quite a few things besides that as well.

As seen in the screenshot, there are a couple of web pages added, but perhaps more interesting are the "PowerCLI" and "Connect VPN" entries.



### Running PowerCLI Core from Royal TSX



I run the [PowerCLI Core Docker container](http://www.virtuallyghetto.com/2016/10/powercli-core-is-now-available-on-docker-hub.html) on my Macbook from time to time, so why not have the option to run it directly from Royal TSX? <del>Once you have it up and running, adding it as a Command Task is pretty easy!</del>

<del>Add a new Command Task, and put in the docker run command in the Command: field</del>

**Update: **Since originally posting, I've discovered that there is an even better ways of doing this, and at the same time keep your PowerCLI running in a tab inside of Royal TSX. Instead of adding it as a Command Task, add a new Terminal connection, but use Custom Terminal as the connection type:

[![](http://vninja.net/wordpress/wp-content/uploads/2017/10/Screenshot-2017-10-16-18.00.39-300x210.png)](http://vninja.net/wordpress/wp-content/uploads/2017/10/Screenshot-2017-10-16-18.00.39.png)

Then add the command you want to run under Custom Commands

[![](http://vninja.net/wordpress/wp-content/uploads/2017/10/Screenshot-2017-10-16-18.01.51-300x213.png)](http://vninja.net/wordpress/wp-content/uploads/2017/10/Screenshot-2017-10-16-18.01.51.png)

In my case, I want to run the following command:

`docker run --rm -it --entrypoint='/usr/bin/powershell' vmware/powerclicore
`

Now, under "Advanced", find the Session option. Enable "_Run inside login shell_" to make sure your applications, like Docker, are found without having to specify the complete path to it, and that's it. As long as Docker runs locally, PowerCLI core can now be launched directly from the navigation bar, and it opens a new tab inside of Royal TSX!

[![](http://vninja.net/wordpress/wp-content/uploads/2017/10/Screenshot-2017-10-16-18.03.27-300x95.png)](http://vninja.net/wordpress/wp-content/uploads/2017/10/Screenshot-2017-10-16-18.03.27.png)

This can also be used to run other things of course, I've added a new Terminal option to my sidebar as well, which opens iTerm2 in a new tab.



### Connecting Tunnelblick VPN Royal TSX



I run OpenVPN at home, and use [Tunnelblick](https://www.tunnelblick.net) as my client of choice. In order to connect to my home network, I've created another Command Task, with the "Run in Terminal" option configured, that runs a simple AppleScript command instructing Tunnelblick to connect.

`osascript -e "tell application \"Tunnelblick\"" -e "connect \"[your-connection-name]\"" -e "end tell"`

I guess I really understated the percentage of awesomeness increase by doing this, _it should at least have been <del>84% </del>**92,7%**_.

https://twitter.com/h0bbel/status/918111617698627584


