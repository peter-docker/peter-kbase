---
type: kbase
version: 3
title: "How do I resolve a '2016/03/09 14:48:23 Registration failed, 402. Retry in 1 seconds' error when starting the Docker Cloud Agent?"
summary: "The Docker Cloud Agent is not able to connect remotely to Docker Cloud. To fix the issue, verify the correct ports are opened in any firewalls. Retry in 1 secondserror occurs when starting the Docker Cloud Agent. 6783/tcp and 6783/udp for the node to join the private overlay network for communicating with containers on other nodes. 2375/tcp: for Docker Cloud to communicate with the Docker daemon running on the node."
dateCreated: "Mon, 14 Mar 2016 22:26:39 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 1
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

### Overview

The Docker Cloud Agent is not able to connect 
remotely to Docker Cloud. To fix the issue, verify the correct ports are opened in any firewalls.

### Symptoms

A`2016``/03/09 14:48:23 Registration failed, 402. Retry in 1``seconds`error occurs when starting the Docker Cloud Agent.

### Resolution

This error may be caused by the host's firewall configuration. V
erify that the firewall has been disabled or that the following ports are opened:

* Required 
    * **6783/tcp** and **6783/udp** for the node to join the private overlay network for communicating with containers on other nodes.
* Recommended 
    * **2375/tcp**: for Docker Cloud to communicate with the Docker daemon running on the node. If port 2375 is not accessible, Docker Cloud will attempt to communicate with node through a secure reverse tunnel.
    * Of course, any ports used by the application will also need to be open.