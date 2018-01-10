---
type: guide
version: 1
title: "Troubleshooting a UCP 2.2.x cluster"
summary: "This article provides direction on how to begin troubleshooting a UCP 2.2.x cluster, as well as the information to gather along the way to allow support to further assist. If the first set of actions does not fix the problem, avoid taking further actions in the same direction, as the issue might have mutated by the action, and the environment may be surfacing a different problem entirely."
dateCreated: "Thu, 14 Sep 2017 16:23:33 GMT"
dateModified: "Wed, 04 Oct 2017 03:18:30 GMT"
dateModified_unix: 1507087110
upvote: 0
internal: no
author: "scasey"
platform:
testedon:
  - ucp-2.2.0
tags:
product:
  - EE
---

This article provides direction on how to begin troubleshooting a UCP 2.2.x cluster, as well as the information to gather along the way to allow support to further assist.

Goal

After completing these steps, you should have a good understanding about how to

1. Begin troubleshooting a UCP 2.2.x cluster
2. Capture important information along the way

## Collect Support Dump

Begin by collecting a support dump, you can mark the exact time the issue occurred and whether it’s persistent/intermittent.

* [/article/What_should_I_include_when_opening_a_support_ticket%3F](/article/What_should_I_include_when_opening_a_support_ticket%3F "/article/What_should_I_include_when_opening_a_support_ticket%3F")

## Verify Backups

Create/verify a backup for all components if possible (UCP, DTR, Swarm, NFS):

* [https://docs.docker.com/datacenter/ucp/2.2/reference/cli/backup/](https://docs.docker.com/datacenter/ucp/2.2/reference/cli/backup/ "https://docs.docker.com/datacenter/ucp/2.2/reference/cli/backup/")
* [https://docs.docker.com/datacenter/dtr/2.3/guides/admin/backups-and-disaster-recovery/](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/backups-and-disaster-recovery/ "https://docs.docker.com/datacenter/dtr/2.3/guides/admin/backups-and-disaster-recovery/")
* [https://docs.docker.com/engine/swarm/admin_guide/#back-up-the-swarm](https://docs.docker.com/engine/swarm/admin_guide/#back-up-the-swarm "https://docs.docker.com/engine/swarm/admin_guide/#back-up-the-swarm")

## Ideas for Investigation

* Container/service status and health
* Container logs and inspect
* Overlay network reachability and DNS resolution
* Resource exhaustion (disk, cpu, memory, io, network, inode)
* Node availability

## Determine Where the Issue May Reside

Strongly related: ![](./images/thumbs_up_green.png) 
Related: ![](./images/thumbs_up_yellow.png)

This matrix describes the relationship between features of Docker EE UCP 2.2.x — this is not exhaustive of all services but rather a starting point for analysis and investigation.

|  | **Controller** | **Swarm Manager** | **auth-api** | **auth-store** | **auth-worker** | **etcd** | **metrics** | **CAs** | **agent/ reconciler** | **engine** |
|---|---|---|---|---|---|---|---|---|---|---|
| Docker API: networks, volumes, containers | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_green.png) |  | ![](./images/thumbs_up_yellow.png) |  | ![](./images/thumbs_up_yellow.png) |  |  |  | ![](./images/thumbs_up_green.png) |
| Docker API: services, secrets, configs | ![](./images/thumbs_up_green.png) |  | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_green.png) |  |  |  |  | ![](./images/thumbs_up_green.png) |
| Authentication Failures | ![](./images/thumbs_up_yellow.png) |  | ![](./images/thumbs_up_green.png) |  | ![](./images/thumbs_up_yellow.png) |  |  |  | ![](./images/thumbs_up_green.png) |  |
| Authorization Failures | ![](./images/thumbs_up_green.png) |  | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_green.png) |  | ![](./images/thumbs_up_green.png) |  |  | ![](./images/thumbs_up_yellow.png) |  |
| Node list timeout/errors | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_green.png) |  |  | |  |  |  |  | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_green.png) |
| Unhealthy nodes | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_yellow.png) |  |  |  |  |  | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_green.png) |
| Node in Pending |  | ![](./images/thumbs_up_yellow.png) |  |  |  |  |  |  | ![](./images/thumbs_up_green.png) |  | |
| “rpc error …” |  |  |  |  |  |  |  |  |  | ![](./images/thumbs_up_green.png) |
| “context deadline exceeded” | ![](./images/thumbs_up_green.png) |  |  |  |  |  |  |  | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_green.png) |
| Charts not loading data | ![](./images/thumbs_up_green.png) |  |  |  |  |  | ![](./images/thumbs_up_green.png) |  | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_green.png) |

## More Troubleshooting

* [https://docs.docker.com/datacenter/ucp/2.2/guides/admin/monitor-and-troubleshoot/troubleshoot-configurations/ ](https://docs.docker.com/datacenter/ucp/2.2/guides/admin/monitor-and-troubleshoot/troubleshoot-configurations "https://docs.docker.com/datacenter/ucp/2.2/guides/admin/monitor-and-troubleshoot/troubleshoot-configurations")

## Review Logs for Errors and Resolution

Review UCP logs based upon the above matrix for errors and hints to resolution.

## Resolve

Capture a support dump after every action taken to fix the problem. Avoid taking any actions unless you fully understand what the problem is. If the first set of actions does not fix the problem, avoid taking further actions in the same direction, as the issue might have mutated by the action, and the environment may be surfacing a different problem entirely. Resolve the issues uncovered from the logs. If unsure, find assistance from [docs.docker.com](http://docs.docker.com "http://docs.docker.com") or [kbase](https://success.docker.com/ "https://success.docker.com") to move forward, collect the data captured during the process, and open a case with [Docker Support](https://support.docker.com "https://support.docker.com").
