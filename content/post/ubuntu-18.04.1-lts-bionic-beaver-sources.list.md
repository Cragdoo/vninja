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
While setting up a new [Ubuntu 18.04.1 LTS Bionic Beaver](http://releases.ubuntu.com/18.04/) VM (which will be used for and upcoming project using Grafana and InfluxDB), I ran into a small issue where the */etc/apt/sources.list* was close to empty:

```
fsociety@test:~$ cat /etc/apt/sources.list
deb http://archive.ubuntu.com/ubuntu bionic main
deb http://archive.ubuntu.com/ubuntu bionic-security main
deb http://archive.ubuntu.com/ubuntu bionic-updates main
fsociety@test:~$
```

Turns out systems that are installed with the live-server installer do not have [universe](https://help.ubuntu.com/community/Repositories/Ubuntu) enabled. I don't know if this is a bug that will be fixed, or an intentional change.

Either way, I've hacked together  my own version (copied from older releases) of a full sources.list file, and made it available as a [gist](https://gist.github.com/h0bbel/4b28ede18d65c3527b11b12fa36aa8d1).

This can easily downloaded into your Ubuntu system by running the following command

```
wget https://gist.githubusercontent.com/h0bbel/4b28ede18d65c3527b11b12fa36aa8d1/raw/a4ab1c13a92171822215143b1e3b3eb6add7a78d/sources.list
```
This should add your familiar repositories back, and enable installation of packages from the missing universe repositories, as well as the main repositories enabled by default.

[Ubuntu 18.04.1 LTS Bionic Beaver /etc/apt/sources.list gist](https://gist.github.com/h0bbel/4b28ede18d65c3527b11b12fa36aa8d1).
