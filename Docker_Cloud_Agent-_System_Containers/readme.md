---
type: kbase
version: 3
title: "Docker Cloud Agent: System Containers"
summary: "Why do I see running containers from images starting withdockercloud/ on my node after installing the Docker Cloud Agent? Once you install the Docker Cloud Agent, you can still use the docker command as you would otherwise. For example, running docker ps will show you a list of containers running. These are system containers that Docker Cloud provisions as part of providing its services."
dateCreated: "Tue, 03 May 2016 20:51:03 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "nhazlett"
platform:
testedon:
tags:
  - Docker Cloud
  - Docker Cloud Agent
product:
  - Hub
---

## Question

Why do I see running containers from images starting with`dockercloud/` on my node after installing the Docker Cloud Agent?

## Answer

Once you install the Docker Cloud Agent, you can still use the `docker` command as you would otherwise. For example, running `docker ps` will show you a list of containers running. **Please note** that there will be containers running from images starting with `dockercloud/` which you may not recognize. These are **system containers** that Docker Cloud provisions as part of providing it's services.
