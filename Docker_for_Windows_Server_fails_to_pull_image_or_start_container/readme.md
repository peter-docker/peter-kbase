---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Docker for Windows Server fails to pull image or start container
internal: no
comment: 
type: kbase
author: yiming
product:
  - ee
testedon:
platform:
  - windows server
tags:
  - daemon
  - error
---
## Issue

When pulling an image or starting a container on an instance of Docker EE running on Windows Server, you may receive any of the following errors:

```
C:\Program Files\Docker\docker.exe: failed to register layer: re-exec error: exit status 1: output: ProcessUtilityVMImage C:\ProgramData\docker\windowsfilter\<id>\UtilityVM: The process cannot access the file because it is being used by another process.
```

```
libcontainerd: failed to start container: container <id> encountered an error during Start: failure in a Windows system call: This operation returned because the timeout period expired. (0x5b4) 
```

## Prerequisites

Before performing these steps, you must meet the following requirements:

- Docker EE for Windows Server (any version)
- Windows Server 2016

## Resolution

When an image pull fails due to any of these errors, this is due to another process currently accessing the directory or files listed in the error. 

This could be antivirus software, an enterprise backup tool, etc, depending on what other process you may have running on your Windows node. If you have trouble identifying the process that is accessing the affected files and/or directories, try using [Sysinternals Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) to search for the locked file or directory name. Also check for any non-system related process accessing it other than `dockerd.exe`.

## What's Next

If the issue is still not resolved by the steps outlined above, please file a support ticket at https://support.docker.com. Docker support engineers will be happy to assist you in resolving the issue.
