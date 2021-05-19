---
layout: post
title: "Running PowerShell scripts"
description: ""
category: 
tags: []
---

#Tip: PowerShell ExecutionPolicy
On my Windows 10, I was trying to run a PowerShell script and kept getting error 
```
vpn1 : File d:\bin\vpn1.ps1 cannot be loaded because running scripts is disabled on this system. For more information,
see about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
```
This happens because no one is allowed to run 
```
PS C:\WINDOWS\system32> Get-ExecutionPolicy
Restricted
```

The default is `Restricted` not allowing anyone to run the scripts. You can set it to `Unrestrcited` but then that is a security risk
```
PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
```

One can make it be a bit more safe by allowing running of scripts written/created by yourself or downloaded from internet but also signed. 
`Set-ExecutionPolicy RemoteSigned`

A bit more safer might be to scope it to `CurrentUser` where only the current user will be allowed to run script.
`Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`

Another option, which I think is even more safer (but may not work for all) is to scope it to current session only. 
`Set-ExecutionPolicy RemoteSigned -Scope Process`


