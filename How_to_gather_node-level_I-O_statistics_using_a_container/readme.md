---
type: kbase
version: 3
title: "How to gather node-level I/O statistics using a container"
summary: "Node-level I/O statistics: In some development and production scenarios, operations teams may want to verify the performance of backend storage networks. This article provides a simple way to gather and report on storage performance on Linux-based nodes using generally available tools. Using appropriate administrative credentials, run the following command on the node you'd like to gather I/O statistics: When complete, kill the container or press Ctrl-C to exit the container."
dateCreated: "Wed, 20 Sep 2017 18:01:28 GMT"
dateModified: "Wed, 04 Oct 2017 03:45:27 GMT"
dateModified_unix: 1507088727
upvote: 0
internal: no
author: bryceryan-docker
platform:
  - linux
testedon:
tags:
product:
  - EE
---

## Issue

**Node-level I/O statistics:** In some development and production scenarios, operations teams may want to verify the performance of backend storage networks. This could be because of intermittent pull failures from the local registry, or intermittent or persistent cluster-wide failures. This article provides a simple way to gather and report on storage performance on Linux-based nodes using generally available tools.

## Prerequisites

Before performing these steps, you must meet the following requirements:

* Linux-based nodes

## Steps

Using appropriate administrative credentials, run the following command on the node you'd like to gather I/O statistics:
```
docker run --rm -it ubuntu sh -c "apt-get update; apt-get install -y sysstat; iostat -dxm 5"
```

When complete, kill the container or press `Ctrl-C` to exit the container.
