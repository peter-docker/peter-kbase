---
type: kbase
version: 4
title: "Windows nodes fail to start after many restarts with HNS error"
summary: "HNS error after restarts"
dateCreated: "Fri, 07 Jul 2017 21:25:54 GMT"
dateModified: "Fri, 29 Sep 2017 20:59:37 GMT"
dateModified_unix: 1506718777
upvote: 0
internal: yes
author: "eiichik"
platform:
  - windows
testedon:
tags:
product:
  - EE
---

After Windows nodes are restarted for multiple times, they fail with following error.

    HNS failed with error : Element not found

## Prerequisites

This issue has been reported on following environment:

* UCP verion 2.1.x with early access for Windows
* Windows Server 2016 worker node with Docker EE engine 17.05

## Steps

To fix this issue, the existing Virtual Switch on Windows has to be recreated using following steps:

1. From the UCP UI, go to **Resources** > **Nodes** and remove the failing node.
2. Connect to the failing Windows node host OS using PowerShell.
3. Remove Virtual Switch using the following command: 
    
        Get-VMSwitch | Where-Object {$_.SwitchType -eq “External”} | Remove-VMSwitch
        Get-ContainerNetwork | Remove-ContainerNetwork
4. Restart the computer: 
    
        Restart-Computer -Force
    
    *Virtual Switch will be recreated after restart.*
5. Go back to UCP UI and go to **Resources** > **Nodes** > **Add** **Node** to get to the swarm join command.
6. Connect to the Windows node again, and execute the join command from step 5.

## What's Next

Microsoft is working on fix to prevent this issue for future releases.