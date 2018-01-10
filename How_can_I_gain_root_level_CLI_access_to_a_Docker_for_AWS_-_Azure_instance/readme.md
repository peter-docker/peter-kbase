---
type: kbase
version: 7
title: "How can I gain root level CLI access to a Docker for AWS / Azure instance?"
summary: "To gain access to the host level of a Docker for AWS or Azure node via SSH, log into the Docker for AWS or Azure node using your Docker username and your private key file. To gain access to the host level of a Docker for AWS or Azure node via the UCP admin client bundle, run the same command as mentioned previously but with a node constraint to specify which node you're targeting."
dateCreated: "Fri, 04 Aug 2017 10:43:31 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 2
internal: no
author: "yiming"
platform:
testedon:
tags:
  - Docker for AWS
  - Docker for Azure
product:
  - EE
---

When a user gains SSH access to a Docker for AWS or Azure node, in actuality the user's access is only within a SSH container deployed on the node. As a result, many non-Docker CLI commands are run within the container itself and not at a host level. Users can run CLI commands at the host level by performing an additional step as outlined in this article.

> **Warning**These steps are provided primarily to assist with debugging. Issuing commands at the root level CLI should be done with caution. Not all configuration changes made at the host level persists across reboots.

## Prerequisites

Before performing these steps, you must meet the following requirements:

* Docker EE cluster deployed via Docker for AWS
* A UCP admin client bundle or SSH access to the nodes in a Docker for AWS cluster

OR

* Docker EE cluster deployed via Docker for Azure
* A UCP admin client bundle or SSH access to the nodes in a Docker for Azure cluster

## Steps

Users can perform either of the following steps to gain access to the host level of a Docker for AWS / Azure node:

### Via SSH

To gain access to the host level of a Docker for AWS or Azure node via SSH, log into the Docker for AWS or Azure node using your Docker username and your private key file. After doing so, start a privileged container to enable shell access on the host using nsenter ([https://github.com/jpetazzo/nsenter](https://github.com/jpetazzo/nsenter "https://github.com/jpetazzo/nsenter")):

    docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh

### Via UCP Admin Client Bundle

To gain access to the host level of a Docker for AWS or Azure node via the UCP admin client bundle, run the same command as mentioned previously but with a node constraint to specify which node you're targeting. For example, if you intend to run it on a node named `node1`:

    docker run -it -e constraint:node==<^>node1<^^> --privileged --pid=host debian nsenter -t 1 -m -u -n -i sh
