---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: "Error: dirperm1 breaks the protection by the permission bits on the lower branch"
internal: no            
comment: ""
type: kbase    
author:  bryceryan-docker
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
  - ee-17.06.2-ee-5
platform:           
  - linux
tags:               
  - daemon
  - error
  - logging
  - monitoring
  - security

---
## Issue

When reviewing the kernel logs relevant to the Docker daemon/engine, the following log message is seen. 
What does it mean? How can I resolve this error?

```
dirperm1 breaks the protection by the permission bits on the lower branch
```

## Prerequisites

- Docker engine later than 1.12-cs

## Resolution

The kernel message about dirperm1 is a warning message from the AUFS that the Docker Engine has enabled 
the `dirperm1` option. This option fixes issues with changing the ownership (chowning) of files from 
a lower layer in image. For Docker's use case, this does not cause a security issue. The AUFS kernel
module displays this message because in some cases, setting the `dirperm1` option is a security risk.

No action for Docker-related issues is required. However, if your node OS makes other uses of AUFS, you 
may want to review the security implications of the `dirperm1` option for those specific uses.

## What's Next

For more background on this message in the context of the Docker engine, see
- [moby/moby 21081](https://github.com/moby/moby/issues/21081)
- [Docker Forums post](https://forums.docker.com/t/what-is-dirperm1-supported/2550/2)
- [Known Issues](https://docs.docker.com/engine/reference/builder/#known-issues-run)
- [moby/moby 783 with workaround](https://github.com/moby/moby/issues/783)
