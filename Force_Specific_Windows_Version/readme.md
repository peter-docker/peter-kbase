---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to force install an older/specific version of Docker EE engine on Windows
internal: no             
comment: ""
type: kbase               
author:  darwintraver
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
  - ee-17.06.2-ee-5
  - ee-17.06.2-ee-3
platform:           
  - windows server
tags:               
  - installing

---
## Issue

How to install a older/specific minor version of Docker EE engine on Windows Server.

## Prerequisites

Before performing these steps, you must meet the following requirements:

- Docker EE Engine 17.0x
- Windows Server 2016

## Resolution

To install a specific older version of Docker EE engine on Windows 2016, use the following steps:

1. The following Powershell command will provide a URL to a JSON file with the current channels available for install:


    ```
    Get-PackageProvider "DockerProvider" | Get-PackageSource
    ```
    
    e.g. https://download.docker.com/components/engine/windows-server/index.json
    
2. To install the current version of the branch, following the documentation instructions:


    ```
    Install-Package -Name docker -ProviderName DockerProvider -Force -MaximumVersion 17.06
    ```
3. To force install an earlier patch/minor version of the Docker engine:


    ```
    Install-Package Docker -ProviderName DockerProvider -Force -RequiredVersion 17.06.2-ee-3 
    ```

## What's Next

Documentation Reference:
[https://docs.docker.com/engine/installation/windows/docker-ee/#install-a-specific-version](https://docs.docker.com/engine/installation/windows/docker-ee/#install-a-specific-version)
