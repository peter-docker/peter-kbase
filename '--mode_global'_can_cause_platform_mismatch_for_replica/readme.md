---
type: kbase
version: 3
title: "'--mode global' can cause platform mismatch for replica"
summary: "When '--mode global' is used, replica will be assigned to all worker nodes even when the container and platform do no match"
dateCreated: "Sat, 19 Aug 2017 07:24:52 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "eiichik"
platform:
  - windows server
  - linux
testedon:
  - ee-17.06.1-ee
tags:
product:
  - EE
---

## Issue

Docker 17.06 EE supports mixed platform worker nodes (for example, Linux and Windows worker nodes in the same cluster).

Starting in Docker 17.06 EE, the Swarm manager automatically schedules containers to worker nodes with the corresponding platform. For example, a service that runs Windows containers will schedule replicas only to Windows worker nodes. However, when `--mode global` is used, task will be assigned to all nodes regardless of the platform type. This might be the desired behavior for all containers.

## Resolution

Task can be assigned a specific platform by adding a `constraint` like this:

    docker service create --name windowsonly `
    --mode global 
    --constraint 'node.platform.os == windows' `
    microsoft/nanoserver
