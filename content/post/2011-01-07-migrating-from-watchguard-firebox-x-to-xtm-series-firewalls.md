---
author: Christian Mohn
comments: true
date: 2011-01-07 22:07:31+00:00
layout: post
slug: migrating-from-watchguard-firebox-x-to-xtm-series-firewalls
title: Migrating from Watchguard Firebox X to XTM Series Firewalls
url: /network/migrating-from-watchguard-firebox-x-to-xtm-series-firewalls/
wordpress_id: 760
categories:
- Network
tags:
- firewall
- Ops
- watchguard
---

Watchguard has recently [retired their X series](http://www.watchguard.com/products/resources/end-of-life-policy.asp) of firewalls and replaced them with their new lineup of XTM boxes.

I took this opportunity to replace my X series firewalls with some from the new lineup, and found a neat way to migrate your existing configuration from old to new in a few  very easy steps.

[![](http://farm6.static.flickr.com/5163/5333692077_fc9c477644_m.jpg)](http://www.flickr.com/photos/h0bbel/5333692077/)

**Note:** _Normally I would not recommend migrating your configuration in this manner. In my mind you should _always_ rebuild rules when replacing your firewall,  as it is the perfect time to review and do some QA._


### 




### Migrating your existing config


**Note:**
I used a laptop do do the actual configuration, to make sure that I didn't get any conflicts in my production environment when setting up the new one with an old config. By default the Watchguard firewalls come with DHCP enabled on eth1 (trusted) and blindly plugging that into your existing infrastructure might not be the best of ideas. Also, remember that the config also includes the firewall IP adress and what happens if you have two firewalls with identical IP adresses in your network? Lets just agree that it's not a pretty sight.


#### Step-by-Step Guide





	
  1. Save your current (old) configuration from your live production firewall

	
  2. Install latest version of Watchguard System Manager

	
  3. Activate new firewall and retrieve feature key from [watchguard.com](http://watchguard.com)

	
  4. Disconnect laptop from existing production environment, and connect it directly to new XTM firewall on eth1 (trusted)

	
  5. Run through Quick Setup Wizard on new XTM firewall

	
  6. Open new config xml file in a suited editor. I used [Notepad++](http://notepad-plus-plus.org/)

	
  7. Find the lines that reads <for-model>x700</for-model>  (your model might differ)

	
  8. Replace x700 with XTM820 (again, your model might differ) and save config file with new name

	
  9. Connect Watchguard System Manager to new firewall and start Policy Manager

	
  10. Open freshly edited config file and save to firebox (if prompted to convert config file to new format, do so)

	
  11. Add new feature key

	
  12. Save to firebox




And there it is. All existing configuration migrated from old Watchguard X series firewall migrated to a new and shiny XTM series. 

You _should_ now be able to do a quick switch between new and old firewalls and all your services should be available immediately. If not, you can always just revert to the old firewall and troubleshoot the new one.


