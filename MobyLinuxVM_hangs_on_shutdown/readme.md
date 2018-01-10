---
type: kbase
version: 4
title: "MobyLinuxVM hangs on shutdown"
summary: "MobyLinuxVM hangs up after receiving a shutdown command"
dateCreated: "Tue, 18 Jul 2017 18:13:25 GMT"
dateModified: "Mon, 24 Jul 2017 19:12:14 GMT"
dateModified_unix: 1500923534
upvote: 0
internal: yes
author: "jasonbivins"
platform:
  - windows  
testedon:
tags:
  - error
product:
  - D4W
---

## Issue

Hyper-V VMs can, on occasion, lose network connectivity with the host, causing it to hang and leading to the host having to be rebooted. This article walks you through ending the MobyLinuxVM without having to reboot the host.

When network connectivity with a VM drops, the host is supposed to immediately re-connect, but sometimes the VM doesn't see the disconnect and holds open the old socket. The host closes the socket on its side and opens a new one. The two are up and online, but completely disconnected from each other. Here is the error you'll see in Docker:

    Unable to stop: Couldn't stop VM MobyLinuxVM
    at Fatal, <No file>: line 376
    at Stop-VM-Force, <No file>: line 372
    at Stop-MobyLinuxVM, <No file>: line 302
    at <ScriptBlock>, <No file>: line 383

## Prerequisites

Before performing these steps, you must meet the following requirements:

* As of this KB - Docker CE 17.06.1
* Windows 10
* Hyper-V

## Steps

Use the following steps to find the MobyLinuxVM PID to kill:

1. If you don't have it already, download Process Explorer from [Sysinternals](https://technet.microsoft.com/en-us/sysinternals/bb896653.aspx?f=255&MSPPError=-2147217396 "https://technet.microsoft.com/en-us/sysinternals/bb896653.aspx?f=255&MSPPError=-2147217396")
2. Find the GUID for the offending VM: 
    1. Check for the GUID name in `C:\ProgramData\Docker\windowsfilter` . Remember or write down at least the first couple and last few characters of the GUID — you're going to need to match.
    2. In Process Explorer look for `VMWP.EXE` and then look under **Command Line** for the process. A parameter on the command will be the GUID from the Virtual Machines folder. Keep looking until you find the one with the same GUID!
3. Stop the VM. After finding it, right-click on that `VMWP.EXE` and choose **Kill Process!** The Virtual Machine should immediately turn off.
4. Restart Docker.

##
