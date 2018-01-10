---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to install/backup DTR when receiving a container wait error
internal: no             
comment: ""
type: kbase               
author:  lenesto.page
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
  - ee-17.06.2-ee-3
  - ucp-2.2.3
  - dtr-2.3.4
platform:           
  - linux
tags:               
  - backup
  - error
  - installing
  - upgrading
---
## Issue

When using a loadbalancer, sometimes the DTR installation, backup, or upgrade is interrupted with the following error: 

```
ERRO[0067] Failed to wait for container: failed to poll wait container 'b46799742906616a25ff6a72551fad6239572dc70fe64171ece7a0fc951e0061': failed to wait for container 'b46799742906616a25ff6a72551fad6239572dc70fe64171ece7a0fc951e0061': Error response from daemon: 504 Gateway Time-out
```

## Resolution

To successfully install, upgrade, or backup DTR after receiving the container wait error use the following steps:

1. Instead of specifying the IP or Hostname for your loadbalancer when constructing your DTR install, upgrade, or backup command for the `ucp-url` flag, use the IP or hostname of the UCP node itself.

## What's Next

This issue is slated to be fixed in DTR 2.3.5.
