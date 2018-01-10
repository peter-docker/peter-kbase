---
type: kbase
version: 5
title: "Hyper-V conflicts with VMware PowerCLI"
summary: "Docker for windows fails to start when VMware PowerCLI is installed"
dateCreated: "Thu, 22 Jun 2017 02:31:58 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "jasonbivins"
platform:
  - windows
testedon:
  - ee-17.03.1-ee-2
tags:
product:
  - D4W
---

## Issue

Docker for Windows fails to start when VMware PowerCLI is installed:
```
    [23:03:37.931][NamedPipeServer][Error  ] Unable to execute Restart: Unable to stop: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: 6/14/2017 11:03:37 PM    Get-VM        You are not currently connected to any servers. Please connect first using a Connect cmdlet.    
    at <ScriptBlock>, <No file>: line 4    at Docker.Backend.HyperV.RunScript(String action, Dictionary`2 parameters)
       at Docker.Backend.ContainerEngine.Linux.DoStop()
       at Docker.Backend.ContainerEngine.Linux.Restart(Settings settings)
       at Docker.Core.Pipe.NamedPipeServer.<>c__DisplayClass8_0.<Register>b__0(Object[] parameters)
       at Docker.Core.Pipe.NamedPipeServer.RunAction(String action, Object[] parameters)
    [23:03:38.601][NamedPipeClient][Error  ] Unable to send Restart: Unable to stop: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: 6/14/2017 11:03:37 PM    Get-VM        You are not currently connected to any servers. Please connect first using a Connect cmdlet.    
    at <ScriptBlock>, <No file>: line 4
    [23:03:38.601][Notifications  ][Error  ] Unable to stop: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: 6/14/2017 11:03:37 PM    Get-VM        You are not currently connected to any servers. Please connect first using a Connect cmdlet.    
    at <ScriptBlock>, <No file>: line 4
```

## Prerequisites

Before performing these steps, you must meet the following requirements:

* Windows 10
* VMware PowerCLI
* Docker 17.03.1 or higher

## Resolution

When you have VMware installed and you use their PowerCLI tools, it changes the syntax of commands that are used when the Engine starts. Specifically within the `MobyLinux.ps1` file. You'll need to fully qualify the cmdlets.

> **Note**: This is a temporary fix. The changes to this file will need to be re-applied every time you re-install or upgrade Docker for Windows.

Edit `C:\Program Files\Docker\Docker\resources\MobyLinux.ps1` to change all cmdlets to qualify them as Hyper-V. **You'll need to find all commands.** Here are two examples: 

* Hyper-V\Get-VM
* Hyper-V\Get-VMHost
