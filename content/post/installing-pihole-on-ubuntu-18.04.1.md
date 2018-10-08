---
type: "post"
draft: "false"
date: "2018-10-08"
year: "2018"
author: Christian Mohn
tags:
- Ubuntu
- Linux
- Blogtober2018
- Pi-Hole

topics: ["Software", "Linux", "Ubuntu", "Blogtober 2018"]

title: "Trouble Installing Pi-Hole on Ubuntu 18.04.1 LTS Bionic Beaver"

description: "Pi-Hole is a nifty little software package that basically acts as a ad-blocking server for your entire network. The installer silently fails on Ubuntu 18.04.1 LTS Bionic Beaver, since it can not install dependencies. That's not good, but hey, there is an easy fix!"

FeaturedImage: "https://vninja.net/img/Pi-Hole_logo.png"

twitter:
card: "summary_large_image"
site: "@vninjanet"
creator: "@h0bbel" 
---

[Pi-Hole](https://pi-hole.net/) is a nifty little software package that acts as a ad-blocking server for your entire network, or as the authors put it:

> Pi-Hole®: A Black Hole for Internet Advertisements.

In reality it's just a local DNS server that blocks out known advertising networks from your queries.

Originally designed to run on a Raspberry Pi (hence the name), it can also run just fine on [any Debian-based linux distribution](https://discourse.pi-hole.net/t/hardware-software-requirements/273), and it works just fine inside a VM. It is very lightweight as it only handles DNS queries and returns a blank HTML file for the blocked requests, it really doesn’t need much processing power.

### Hardware requirements
* ~52MB of free space
* 512 MB RAM

Installation of Pi-Hole is normally pretty straight forward, if you like to [install things directly off the internet](https://sandstorm.io/news/2015-09-24-is-curl-bash-insecure-pgp-verified-install) that is...

Once you have your VM ready, with a static IP, SSH into it and run:

{{< highlight bash >}}curl -sSL https://install.pi-hole.net | bash{{< /highlight >}}

In my case, on Ubuntu 18.04.1 LTS Bionic Beaver, this seemed to run fine, but after a very quick screen flash, it just stopped, looking like this:

{{< highlight bash >}}
fsociety@test:/$ curl -sSL https://install.pi-hole.net | bash

  [✗] Root user check
  [i] Script called with non-root privileges
  [i] The Pi-hole requires elevated privileges to install and run
  [i] Please check the installer for any concerns regarding this requirement
  [i] Make sure to download this script from a trusted source

  [✓] Sudo utility check

  [✓] Root user check

        .;;,.
        .ccccc:,.
         :cccclll:.      ..,,
          :ccccclll.   ;ooodc
           'ccll:;ll .oooodc
             .;cll.;;looo:.
                 .. ','.
                .',,,,,,'.
              .',,,,,,,,,,.
            .',,,,,,,,,,,,....
          ....''',,,,,,,'.......
        .........  ....  .........
        ..........      ..........
        ..........      ..........
        .........  ....  .........
          ........,,,,,,,'......
            ....',,,,,,,,,,,,.
               .',,,,,,,,,'.
                .',,,,,,'.
                  ..'''.

  [✓] Disk space check

  [✓] Update local cache of available packages

  [✓] Checking apt-get for upgraded packages... up to date!

  [i] Installer Dependency checks...
  [✓] Checking for apt-utils
  [i] Checking for dialog (will be installed)
  [✓] Checking for debconf
  [i] Checking for dhcpcd5 (will be installed)
  [✓] Checking for git
  [✓] Checking for iproute2
  [✓] Checking for whiptail
fsociety@test:/$
{{< /highlight >}}
Well, That is strange. It just stopped, no error messages or anything. Turns out the reason it stopped here is because the **dialog** and **dhcpd5** packages are missing from the system and it is unable to install them, even if it does say that they "will be installed". 

[Dialog](https://www.linuxjournal.com/article/2807) is basically a utility that lets you build user interfaces for scripts, and guess what? The Pihole installer is based on it being available. Since installations of Ubuntu 18.04.1 LTS Bionic Beaver using the [LiveCD ISO is missing the Universe repositories](https://vninja.net/2018/10/08/ubuntu-18.04.1-lts-bionic-beaver-sources.list/), the Pi-Hole installer is unable to install its requirements, and just stops in its tracks. 

<!--Jumbotron-->
<div class="jumbotron jumbotron-fluid">

    <div class="container">
        <p class="lead"><i class='fa fa-exclamation-circle'></i> To get any further, you will have to <a href="https://vninja.net/2018/10/08/ubuntu-18.04.1-lts-bionic-beaver-sources.list/">fix your sources.list</a> to make sure you have the Universe repositories and re-run the installer afterwards.</p>
    </div>

</div>
<!--Jumbotron-->

Once that is done, re-run the Pi-Hole installer and it should run just fine, presenting you with a screen similar to this:

![Pi-Hole](/img/pi-hole1.png#center)

From there, you can go on and confgure it to suit your setup. 

I will not go through the configuration of Pi-Hole in this post, I will save that for a later post where I go through how it is configured in my home network, with internal upstream DNS servers. My setup also makes this my default DNS server for VPN connections, making me ad-blocked while mobile as well. **Awesome**.

I will say this though, this thing works extremely well:

![Pi-Hole Admin Dashboard](/img/pi-hole2.png#center)

That's nearly 24% of all requests from my home network blocked before it hits the internet. That is a lot!