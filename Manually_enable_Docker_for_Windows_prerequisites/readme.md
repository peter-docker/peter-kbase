---
type: kbase
version: 6
title: "Manually enable Docker for Windows prerequisites"
summary: "How to manually enable windows pre-reqs"
dateCreated: "Thu, 22 Jun 2017 14:19:12 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "jasonbivins"
platform:
  - windows
testedon:
tags:
  - installing
product:
  - D4W
---

When installing Docker for Windows, occasionally there are a few extra setup steps that Windows needs to run during setup. Even when run as an Administrator, the installer doesn't always allow these extra steps to run, and they need to be applied manually. Please be advised that this is intended for new installs as it will delete any existing containers or images.

For reference here are the prerequisites: [https://docs.docker.com/docker-for-w...re-you-install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install "https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install")

> **Warning:** You will lose any Docker containers or images. As always, please run a full backup before making any changes.

These steps can be done through the Windows control panel or by using the below Powershell commands with elevated privileges:

1. Uninstall Docker, Hyper-V, and, if installed, the Windows containers feature.
2. Reboot.
3. Enable Hyper-V:  
    ```
    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
    ```
4. Reboot.
5. Enable the containers feature:  
    ``
    Enable-WindowsOptionalFeature -Online -FeatureName Containers -All
    ```
6. Install Docker for Windows.
