---
author: cmohn
comments: true
date: 2012-07-24 23:31:12+00:00
layout: post
link: http://vninja.net/osx/applescript-desktop-wallpaper-growl-automatically/
slug: applescript-desktop-wallpaper-growl-automatically
title: Applescript, Desktop Wallpaper and Growl. Automagically.
wordpress_id: 1927
categories:
- OS X
tags:
- Applescript
- automation
- fun
- Mac
- OS X
- Scripting
---

I´ve been a Mac and OS X user now for about 7 months, but to be honest I have not been very experimental in my usage of my Macbook Air. Until now, my main focus has been basic usage of the hardware and operating systems, and making it work in my corporate environment. As I feel that I´ve reached mission accomplished status on that initial project, I decided to use some of my "free" vacation time to play around a bit and experiment a bit more.

For some reason, I decided to start to play around a bit with Applescript and try to create something that changes my desktop wallpaper based on a "Home" and a "Work" scenario. Initially I made a quick script, my first ever Applescript, that presented a dialog box asking me if "Home" or "Work" was the current status, and then changed the desktop wallpaper accordingly. I then assigned that to a hotkey, so I could quickly change between the two modes.

That solution left me with two things; _One, it worked!_ and Two, it could be done better by adding some automation to it.

Here is the solution I ended up with:

[cc lang="applescript" tab_size="2"]
# Populate ipaddr variable with IP address
set ipaddr to do shell script "ifconfig | awk '/broadcast/ {print $2}' | tail -1"

# Check if connection is home connection, based on IP subnet

# If not connected, set ipaddr variable to 0.0.0.0 to avoid empty variable issues
# Dirty hack to add padding to zero values, but hey, it works for me.

if (ipaddr is "") then
set ipaddr to "000.000.0.0"
end if

set ipaddrtxt to (characters 1 thru 9 of ipaddr) as string
if ipaddrtxt is "192.168.5" then # Change this to your own subnet
set situation to "Home"
else
set situation to "Work"
end if

# Get current desktop image path
tell application "System Events"
tell current desktop
set currentDesktop to (get picture as text)
end tell
end tell

# Build the desktop image path
set desktopPath to "Macintosh HD:Users::Dropbox:switcher:" & situation & ".png" # Change this to your own paths

# Only tell Finder to change the desktop and notify Growl if a change is required (eg. desktop path not equal to current path)
if currentDesktop is not desktopPath then

# Tell Finder to change the desktop
tell application "Finder"
set desktop picture to {desktopPath} as alias
end tell

tell application "GrowlHelperApp"
set the allNotificationsList to {"Wallpaper Change"}
set the enabledNotificationsList to {"Wallpaper Change"}
register as application "Wallpaper Change" all notifications allNotificationsList default notifications enabledNotificationsList icon of application "Script Editor"
-- Send a Notification...
notify with name "Wallpaper Change" title "Wallpaper Change" description "Wallpaper changed to " & (situation) application name "Wallpaper Change"
end tell
end if
[/cc]

I know my theme isn´t very suited for code pastes, so the whole applescript source code is also available for [download](https://dl.dropbox.com/u/20629/vninja/Switcher.scpt).

After the script was finished, I added it to my [Geektools](http://projects.tynsoe.org/en/geektool/) bash script, that refreshes at a fairly frequent interval:

[cc lang="bash" tab_size="2"]
# Run Background Switcher Applescript
osascript /Applescript/Switcher.scpt
[/cc]

Sure, I could have used a crontab instead, but for me the easiest way to get this working quickly was to add it to the existing setup I had in place.



### Demo time!




The Applescript detects if the computers IP address is on a given subnet, and if so it sets the wallpaper specified for that subnet, dubbed "Home". If it determines that it is not on that particular subnet, it´s officially at "Work" and sets the wallpaper accordingly. That´s all the logic there is too it, and I´m sure that can be improved either by me or more likely someone who has been hit by the Applescript clue bat.

My desktop Wallpaper images are stored in a given folder in my Dropbox, with one of them called _Home_ and the other called _Work_. This limits the script to those two given file names, and switches between the two accordingly. It also assumes the the files are on the _PNG_ format. An added bonus by storing the files in Dropbox is that I can remotely change out the image, and keep the wallpapers synchronized between several computes if I wish to do so.

Note that I am under no circumstances a developer and that this is my first Applescript. I am sure there are much better ways to accomplish the same things, with better error handling and overall better code quality, but the fact is that this works for me and I´m happy with it.

