---
type: kbase
version: 3
title: "Docker for Windows freezes with invalid daemon settings"
summary: "Docker for Windows will let you add invalid configuration settings, but is unuseable until you manually recover it."
dateCreated: "Fri, 28 Jul 2017 15:45:20 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "jasonbivins"
platform:
  - windows
testedon:
tags:
  - daemon
  - error
product:
  - D4W
---

## Issue

Docker for Windows gives you the ability to configure the Docker daemon through the GUI. Normally it will warn you if there are invalid settings, but occasionally it might let you proceed with a bad configuration. When that happens - Docker hangs up on restarting and cannot be started again even if you manually terminate it. This article walks you through recovering Docker. ***WARNING - You will lose data. Any Daemon changes or shared drive settings will be reset.***

The following example includes an invalid configuration change that Docker accepted. Docker is hung on restarting and eventually errors out:

![](./images/restarting_stuck.png)

Eventually, Docker will crash with this message.

![](./images/daemon_errror.png)

Even restarting your computer won't help - Docker still has the invalid config and can't start:

![](./images/red_whale.png)

## Resolution

To fix the invalid setting and restart Docker for Windows, use the following steps:

***WARNING - You will lose data. Any Daemon changes or shared drive settings will be reset.***

1. Stop the Docker for Windows service: 
    
    ![](./images/Docker_for_windows_service.png)
2. Stop the `Docker for Windows.exe` process: 
    
    ![](./images/Docker_for_windows_exe.png)
3. Navigate to `C:\Users\Username\AppData\Roaming\Docker` and either delete or rename the `settings.json` file. 
    
    ![](./images/settings.config.png)
4. Restart Docker — Your system will recognize that the Docker for Windows service is not running and will pop-up a window that allows you to start it. It will also recreate the `settings.json` file based on the default template.
