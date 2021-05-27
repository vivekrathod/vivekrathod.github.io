---
title: Change hostname on Ubuntu
date: 2021-05-25 00:00:00 Z
description: 
layout: post
---

## Check current hostname
Simply run `hostname` or `hostnamectl` command, or simply `echo "$HOSTNAME"` ...or look into `cat /etc/hostname`

## Change hostname
* First `sudo hostnamectl set-hostname newNameHere`
* Then edit `/etc/hosts` with `sudo nano /etc/hosts`
* Reboot 'sudo reboot' for changes to take effect
