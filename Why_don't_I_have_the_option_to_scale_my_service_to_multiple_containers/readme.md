---
type: kbase
version: 3
title: "Why don't I have the option to scale my service to multiple containers?"
summary: "Has it been moved somewhere or have I misconfigured something and hence I don't have the option to scale the service to multiple containers? If the service is linked from another service with EVERY_NODE strategy, containers will be linked one-to-one on each node. Note: Currently it is not possible to change from EVERY_NODE to HIGH_AVAILABILITY or EMPTIEST_NODE and vice versa, so a new service would need to be created in order to use the new deployment strategy."
dateCreated: "Thu, 31 Mar 2016 16:37:19 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "nhazlett"
platform:
testedon:
tags:
  - Docker Cloud
product:
  - Hub
---

## Question

I just configured a service and cannot find the "number of containers" slider that allows scaling. Has it been moved somewhere or have I misconfigured something and hence I don't have the option to scale the service to multiple containers?

## Answer

Check if the EVERY_NODE deployment strategy was selected when creating the service, since using this deployment strategy prevents manual scaling. A service with an EVERY_NODE strategy will have one container deployed on each node that matches its deploy tags. It has the following properties:

* With every new node that matches the service’s deploy tags, a new container will be deployed to it.
* **It cannot be manually scaled.**
* If the service uses volumes, each container on each node will have a different volume.
* If the service is linked from another service with EVERY_NODE strategy, containers will be linked one-to-one on each node.

Note: Currently it is not possible to change from EVERY_NODE to HIGH_AVAILABILITY or EMPTIEST_NODE and vice versa, so a new service would need to be created in order to use the new deployment strategy.
