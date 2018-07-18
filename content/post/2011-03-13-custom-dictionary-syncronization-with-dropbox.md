---
author: Christian Mohn
comments: true
date: 2011-03-13 21:29:42+00:00
layout: post
slug: custom-dictionary-syncronization-with-dropbox
title: Custom Dictionary Syncronization with Dropbox
url: /howto/custom-dictionary-syncronization-with-dropbox/
wordpress_id: 920
categories:
- Howto
tags:
- Dropbox
- Microsoft Office
- Productivity
- Tips &amp; Tricks
---

In the last couple of weeks I've been using Microsoft Word 2010 a lot more than I've previously done, and at the same time I've been switching computers a lot making it somewhat of an annoyance that a lot of the words I use in my documents are not recognized and marked with a wiggly red line underneath it. 

_If only there was a way to keep the custom dictionary synchronized between computers._

**And guess what? There is. To the cloud!**

Setting up custom dictionary synchronization between several computers is pretty easy to set up, at least if you have a [Dropbox](http://www.dropbox.com/) account. If you don't, sign up now via [this affiliate link](http://db.tt/bRSrPaI) to give me some extra space to play with at the same time.





  1. Find your existing custom.dic file. Custom.dic is the local file Microsoft Office keeps your custom additions to the standard dictionary in. Usually it's located in _C:\Users\{username}\AppData\Roaming\Microsoft\UProof_.


  2. Copy the file to a location within your Dropbox folder structure. I've chosen _My Dropbox\Applications\Office_


  3. I renamed the file to myCustom.dic to differenciate it from the local one, but that's not required


  4. Configure Microsoft Word 2010 to use the Dropbox syncronized file by going to **File -> Options - Proofing** and clicking on the "**Custom Dictionaries...**" button.
  

[![](/img/Custom-Dictionaries-across-Computers01-300x244.png)](/img/Custom-Dictionaries-across-Computers01.png)
This brings up a new window where you can change your custom dictionary file location. Click on "**Add**"

[![](/img/Custom-Dictionaries-across-Computers02-300x124.png)](/img/Custom-Dictionaries-across-Computers02.png)

Browse to your Dropbox located custom.dic file and add it.

[![](/img/Custom-Dictionaries-across-Computers03-300x208.png)](/img/Custom-Dictionaries-across-Computers03.png)

Now you have two custom dictionaries selected. I normally opt for using just one to make sure I get all my additions synchronized. Do your changes, and click on "**Ok**"

[![](/img/Custom-Dictionaries-across-Computers04-300x124.png)](/img/Custom-Dictionaries-across-Computers04.png)


  5. Repeat _step 4_ the different computers you use, and the custom dictionary will automatically sync between them. Of course, this requires that you have Dropbox installed as well.


  6. Close the "Word Options" window, and enjoy life with synchronized custom dictionaries in Microsoft Office 2011.

