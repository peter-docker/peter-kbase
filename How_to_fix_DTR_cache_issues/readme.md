---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Troubleshooting common DTR content cache issues
internal: no
comment: 
type: kbase
author: yiming
product: 
  - ee 
testedon: 
  - dtr-2.2.0
  - dtr-2.3.0
platform: 
  - linux
tags: 
  - error
---
## Issue

DTR caches can be set up in a separate geographic location than where the DTR cluster is located. By deploying caches geographically closer to remote offices and low connectivity areas, users can pull images faster.

This article covers basic troubleshooting steps should a DTR user configured to pull from a DTR cache run into errors while pulling images.

## Prerequisites

Before performing these steps, you must meet the following requirements:

- The user must be configured to use a DTR content cache. For more information on how to do so, follow the steps [in the Docker docs](https://docs.docker.com/datacenter/dtr/2.3/guides/user/access-dtr/use-a-cache/).
- The DTR version must be 2.2.0 or newer, and already have DTR caches created by an administrator.

## Resolution

To identify the cause of an issue with DTR pulls, check on the state of the DTR cache container and the DTR cache logs:

1. Check the process.
    ```
    $ docker ps -a | grep dtr-cache
    ```
2. Check the logs.
    ```
    $ docker logs dtr-cache
    ```
3. From the first two steps, identify if the container is in a restarting state.

If the container is restarting, and the error displayed shows the error 

```panic: unable to configure registry middleware (downstream): error parsing upstreams: error casting upstreams array```

this typically suggests that a DTR 2.2.x cache was used in tandem with a DTR 2.3.x configuration format. To resolve this, you need to redeploy the DTR cache using the [2.2.x configuration syntax](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/configure/deploy-caches/#deploy-a-simple-cache).

If the container is running, but the logs contain the error 

```error unmarshaling response from upstream https://dtr-url: invalid character '-' in numeric literal```

the error is due to using an incorrect DTR cache version as opposed to the current DTR version. You need to redeploy the DTR cache using the same version tag as DTR.

## What's Next

- [Creating a DTR cache for DTR 2.3.x](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/configure/deploy-caches/)
- [Deploy a DTR cache with TLS for DTR 2.3.x](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/configure/deploy-caches/tls/)
- [Chain multiple DTR caches for DTR 2.3.x](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/configure/deploy-caches/chaining/)
