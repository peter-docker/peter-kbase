---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to force remove an orphaned task from a network
internal: no             
comment: ""
type: kbase              
author:  darwintraver
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
  - ee-17.06.2-ee-4
  - ucp-2.2.3
platform:           
  - linux
tags:               
  - daemon
  - error
  - networking

---
## Issue

When attempting to remove a Docker network (`docker network rm`), the following error is presented:

```
Error response from daemon: Error response from daemon: rpc error: code = 9 desc = network <^><network id><^^> is in use by task <^><task id><^^>
```

## Prerequisites

Before performing these steps, you must meet the following requirements:

- Universal Control Plane 2.1.x and higher
- Docker Engine 17.03.x - 17.06.02-ee4

## Resolution

To remove the orphaned task that is preventing the Docker network from being removed, use the following steps:

1. Confirm there is no attachable containers on the network.
2. Pull down the tasknuke tool:

    ```
   docker pull dperny/tasknuke
    ```
3. Using the tasknuke tool, remove the orphaned task:

    ```
    docker run -v /var/run/docker/swarm/control.sock:/var/run/swarmd.sock dperny/tasknuke <^><taskid><^^>
    ```

## What's Next

This has been patched in Docker 17.06.02-ee5+. If you still experience an issue in latest versions (ee5/ee6), please contact Docker Support.

- [Release notes:](https://docs.docker.com/enterprise/17.06/#17062-ee-5-2017-11-02) _"When a node is removed, delete all of its attachment tasks so networks used by those tasks can be removed"_

- [Github pull request](https://github.com/docker/swarmkit/pull/2414)
