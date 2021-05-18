---
layout: post
title: "Configure GUI apps on WSL"
description: ""
category: 
tags: []
---

# Running GUI apps from WSL2 

## Install X Server
I chose[ VcXsrv](https://sourceforge.net/projects/vcxsrv/) I do not like installing software on Windows so I chose to use portable version. To do that I downloaded the regular setup and used 7-zip to extract the contents of the 'setup'. There are some instructions [here](https://sourceforge.net/p/vcxsrv/wiki/Making%20Portable/) Delete the $PLUGINSDIR folder.

## Configure VcXsrv
Follow instructions to configure VcXsrv [here](https://sourceforge.net/p/vcxsrv/wiki/Using%20VcXsrv%20Windows%20X%20Server/)
You can run *xlaunch.exe* to configure how to run *VcXsrv* and store the configuration data in a file. One can then launch it this way
`vcxsrv.exe" -run config.xlaunch`
I simply run it with
`vcxsrv.exe -multiwindow -clipboard -wgl -ac`
The `-ac` switch tunrs-off the access control. Read more about other switches [here](https://sourceforge.net/p/vcxsrv/wiki/Using%20VcXsrv%20Windows%20X%20Server/) There are ways to do this more securely by adding authorization keys to `.Xauthority` , but I have not tried - refer to [this page](https://sourceforge.net/p/vcxsrv/wiki/VcXsrv%20&%20Win10/) to learn more.
You can also create a batch file or a desktop/taskbar shortcut - something like
`D:\bin\VcXsrv\vcxsrv.exe -multiwindow -clipboard -wgl -ac`

## Configure WSL2
Update and upgrade and install desktop environment of your choice - I chose *xfce*
`:~$sudo apt update && sudo apt upgrade -y && sudo apt install xfce4`

Then export the DISPLAY environmental variable and point it to your X server which for me is VcXsrv running on my host Windows OS
```
~$ export DISPLAY=$(/sbin/ip route | awk '/default/ { print $3 }'):0
~$ echo $DISPLAY
192.168.165.113:0
:~$ nc -v 192.168.165.113 6000
Connection to 192.168.165.113 6000 port [tcp/x11] succeeded!
^C
```
If at the last step connection to TCP port 6000 fails then you would need to add a firewall rule to allow VcXsrv on your Windows host.
At this point running any X application like `xclock` should open the window on your host Windows OS.
You can add this to bash profile so that it is automatically run everytime you open WSL
`:~$ echo "export DISPLAY=$(/sbin/ip route | awk '/default/ { print $3 }'):0" >> ~/.bashrc`