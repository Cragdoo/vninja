---
type: "post"
draft: "false"
date: "2018-09-25"
year: "2018"
author: Christian Mohn
topics: ["macOS", "Software", "Homebrew"]
tags: ["Homebrew", "macOS"]

title: "macOS: Homebrew error in macOS 10.14 Mojave"

description: "How to fix **Error: Git must be installed and in your PATH!** with Homebrew in macOS 10.14 Mojave."

FeaturedImage: "https://vninja.net/img/homebrew-256x256.png"

twitter:
card: "summary_large_image"
site: "@vninjanet"
creator: "@h0bbel" 
---
Just a quick note to everyone jumping on the new [macOS Mojave release](https://notesfrommwhite.net/2018/09/24/macos-mojave-now-ga/). 

I'm a big fan of [Homebrew](https://brew.sh), and after upgrading macOS I figured it would be a good idea to upgrade Homebrew as well.

```
 ~ $ brew update
==> Downloading https://homebrew.bintray.com/bottles/git-2.18.0.mojave.bottle.ta
...
==> Summary
🍺  /usr/local/Cellar/git/2.18.0: 1,488 files, 296.7MB
Error: Git must be installed and in your PATH!
```

Well, that's strange, Homebrew was working just fine before the upgrade. 

Turns out that the Mojave upgrade didn't also upgrade my Xcode Command Line tools, which causes this error when upgrading Homebrew. 

Luckily there is a really quick fix for this; just install the Xcode command line tools!

```
 ~ $ xcode-select --install
xcode-select: note: install requested for command line developer tools
```

Let that run, and Homebrew upgrades itself without problems.

```
 ~ $ brew update
Updated 2 taps (homebrew/core, homebrew/cask).
```

I'm expecting some other upgrades to Homebrew in soonish, since it still considers Mojave (macOS 10.14) to be a pre-release version so official support isn't quite there yet.