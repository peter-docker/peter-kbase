---
type: kbase
version: 3
title: "High Memory Usage by dockerd leads to 'Out of Memory'"
summary: "A bug was identified in the Docker platform, specifically in the built-in swarm-mode orchestration, which can lead to high memory usage from the Docker daemon and out of memory issues on a node. In the meantime, the workaround is to restart the Docker daemon on the affected node in order to temporarily return memory to the system."
dateCreated: "Tue, 20 Jun 2017 14:29:05 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "tfoxnc"
platform:
testedon:
  - cse-1.13.1-cs1
  - ee-17.03.0-ee-1
tags:
product:
  - EE
---

## Issue

A bug was identified in the Docker platform, specifically in the built-in swarm-mode orchestration, which can lead to high memory usage from the Docker daemon and out of memory issues on a node. In particular, a manager node that loses leadership during a RAFT election may have cluster events stack up in the events queue, leading to high memory usage and eventual out of memory issues.

More specific details on the issue and fix can be found in the Docker Swarmkit github page: <https://github.com/docker/swarmkit/pull/2215>

Updates are available for Docker versions 1.13, 17.03 and subsequent releases.

## Impact

This bug affects Docker versions 1.13 and 17.03 when running a swarm-mode cluster, including Docker Enterprise Edition clusters using Universal Control Plane. Updates will be made available to these version.

Universal Control Plane and Docker Trusted Registry do not require separate updates, and you can continue to use existing versions of these components. Only the underlying Docker EE Engine (previously CS Engine) requires updates to fix this issue.

## Mitigation

Docker is making available patches for 1.13 and 17.03, and the problem will be fixed in subsequent releases.

In the meantime, the workaround is to restart the Docker daemon on the affected node in order to temporarily return memory to the system.

Please contact Docker Support if you have any questions or concerns.