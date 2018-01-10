---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to install, backup, or upgrade DTR when receiving a container wait error
internal: no             
comment: ""
type: kbase          
author:  lenesto.page
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:
  - ee-17.06.2-ee-3
  - ucp-2.2.3
  - dtr-2.3.4
platform:           # Optional. Keep all that apply.
  - linux
tags:            
  - backup
  - error
  - installing
  - upgrading
---
## Issue

This article explains how to install, backup, or upgrade DTR when receiving a container wait error. Several customers, using a loadbalancer, have had their installation, backup, or upgrade interrupted with the following error: 

```
ERRO[0067] Failed to wait for container: failed to poll wait container 'b46799742906616a25ff6a72551fad6239572dc70fe64171ece7a0fc951e0061': failed to wait for container 'b46799742906616a25ff6a72551fad6239572dc70fe64171ece7a0fc951e0061': Error response from daemon:
504 Gateway Time-out
504 Gateway Time-out
```
## Prerequisites

Before performing these steps, you must meet the following requirements:

- Customer must be using a loadbalancer as this error most likely occurs because of the loadbalancer

To successfully install, upgrade, or backup DTR after receiving the container wait error please see below:

1. Instead of specifying the IP or Hostname for your loadbalancer when constructing your DTR install, upgrade, or backup command for the `ucp-url` flag, use the IP or hostname of the UCP node itself.

## What's Next

This issue is slated to be fixed in DTR 2.3.5.
