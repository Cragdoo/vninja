---
title: "Ubuntu 18.04.1 LTS Bionic Beaver Missing Entries in sources.list"
date: 2018-10-08T10:19:23+02:00
draft: true
type: "post"
draft: "false"
date: "2018-10-08"
year: "2018"
author: Christian Mohn
tags:
- Ubuntu
- Linux
- Blogtober2018

topics: ["Software", "Linux", "Ubuntu", "Blogtober 2018"]

description: "While setting up a new Ubuntu 18.04.1 LTS Bionic Beaver VM (which will be used for Grafana and InfluxDB), I ran into a small issue where the */etc/apt/sources.list* was close to empty"

FeaturedImage: "https://vninja.net/img/ubuntu-logo112.png"

twitter:
card: "summary_large_image"
site: "@vninjanet"
creator: "@h0bbel"
---
While setting up a new [Ubuntu 18.04.1 LTS Bionic Beaver](http://releases.ubuntu.com/18.04/) VM (which I will be using for a couple of upcoming projects using Grafana and InfluxDB, as well as [Pi-Hole](/2018/10/08/installing-pihole-on-ubuntu-18.04.1/)), I ran into a small issue where the */etc/apt/sources.list* was close to empty:

{{< highlight bash >}}
fsociety@test:~$ cat /etc/apt/sources.list
deb http://archive.ubuntu.com/ubuntu bionic main
deb http://archive.ubuntu.com/ubuntu bionic-security main
deb http://archive.ubuntu.com/ubuntu bionic-updates main
fsociety@test:~$
{{< /highlight >}}

*That does seem a bit on the too scaled-down side, doesn't it?*

Turns out systems that are installed with the Ubuntu 18.04.1 LTS Bionic Beaver [live server installer](https://help.ubuntu.com/lts/serverguide/installing-live-server.html.en) does not have all the [normal Ubuntu repositories](https://help.ubuntu.com/community/Repositories/Ubuntu) added to it. In fact, **Universe**, **Multiverse** and **Restricted** repositories are missing. 

I don't know if this is a bug that will be fixed, or an intentional change, but it means that a lot of packages you might need are not available for installation after completing the Ubuntu OS setup. A*nd that is simply no fun at all*.

Either way, I've hacked together my own version (basically I've just copied it from older releases) of a full sources.list file, and made it available as a [gist](https://gist.github.com/h0bbel/4b28ede18d65c3527b11b12fa36aa8d1).

This can easily downloaded into your Ubuntu 18.04.1 LTS Bionic Beaver system after a backup of the original one:

### Backup the original */etc/apt/sources.list*
{{< highlight bash >}}
fsociety@test:~$ sudo mv /etc/apt/sources.list /etc/apt/sources.list.backup
{{< /highlight >}}

### Download my updated */etc/apt/sources.list*
{{< highlight bash >}}
fsociety@test:~$ cd /etc/apt/
fsociety@test:~$ sudo wget https://gist.githubusercontent.com/h0bbel/4b28ede18d65c3527b11b12fa36aa8d1/raw/a4ab1c13a92171822215143b1e3b3eb6add7a78d/sources.list
{{< /highlight >}}

This should add your familiar repositories back, and enable installation of packages from the missing repositories, as well as the main repositories enabled by default. You can test it by running a simple apt-get update command:

{{< highlight bash >}}
sudo apt-get update
[sudo] password for fsociety:
Get:1 http://security.ubuntu.com/ubuntu bionic-security InRelease [83.2 kB]
Hit:2 http://us.archive.ubuntu.com/ubuntu bionic InRelease
Get:3 http://us.archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
Get:4 http://us.archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages [398 kB]
Get:5 http://us.archive.ubuntu.com/ubuntu bionic-updates/main Translation-en [149 kB]
Get:6 http://us.archive.ubuntu.com/ubuntu bionic-updates/restricted amd64 Packages [7024 B]
Get:7 http://us.archive.ubuntu.com/ubuntu bionic-updates/restricted Translation-en [3076 B]
Get:8 http://us.archive.ubuntu.com/ubuntu bionic-updates/universe amd64 Packages [559 kB]
Get:9 http://us.archive.ubuntu.com/ubuntu bionic-updates/universe Translation-en [145 kB]
Get:10 http://us.archive.ubuntu.com/ubuntu bionic-updates/multiverse amd64 Packages [5716 B]
Get:11 http://us.archive.ubuntu.com/ubuntu bionic-updates/multiverse Translation-en [3176 B]
Fetched 1441 kB in 3s (532 kB/s)
Reading package lists... Done
{{< /highlight >}}

And there you go, **Universe**, **Multiverse** and **Restricted** repositories are now available, in addition to default **Main**. 

Happy days!

<!--Jumbotron-->
<div class="jumbotron jumbotron-fluid">

    <div class="container">
        <p class="lead"><i class='fa fa-exclamation-circle'></i> <a href="https://gist.github.com/h0bbel/4b28ede18d65c3527b11b12fa36aa8d1">Ubuntu 18.04.1 LTS Bionic Beaver /etc/apt/sources.list gist</a></p>
    </div>

</div>
<!--Jumbotron-->