---
layout: post
title: "Change hostname on Ubuntu"
description: ""
category: 
tags: []
---
## Check current hostname
Simply run `hostname` or `hostnamectl` command, or simply `echo "$HOSTNAME"` ...or look into `cat /etc/hostname`

## Change hostname
* First `sudo hostnamectl set-hostname newNameHere`
* Then edit `/etc/hosts` with `sudo nano /etc/hosts`
* Reboot 'sudo reboot'
