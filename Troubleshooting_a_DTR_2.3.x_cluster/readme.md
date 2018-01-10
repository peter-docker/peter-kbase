---
type: guide
version: 3
title: "Troubleshooting a DTR 2.3.x cluster"
summary: "This article provides direction on how to begin troubleshooting a DTR 2.3.x cluster, as well as the information to gather along the way to allow Docker Support to further assist. If the first set of actions does not fix the problem, avoid taking further actions in the same direction, as the issue might have mutated by the action, and the environment may be surfacing a different problem entirely."
dateCreated: "Thu, 14 Sep 2017 17:38:00 GMT"
dateModified: "Wed, 04 Oct 2017 03:24:37 GMT"
dateModified_unix: 1507087477
upvote: 0
internal: no
author: "scasey"
platform:
  - linux
testedon: 
  - dtr-2.3.0
tags:
product:
  - EE
---

This article provides direction on how to begin troubleshooting a DTR 2.3.x cluster, as well as the information to gather along the way to allow Docker Support to further assist.

## Prerequisites

User should understand:

* DTR architecture
* How to view logs
* How to access Engine logs in debug mode

## Goal

After completing these steps, you should have a good understanding how to:

* Begin troubleshooting a DTR 2.3.x cluster
* Capture important information along the way

## Collect Support Dump

Begin by collecting a support dump. You can mark the exact time the issue occurred and whether it’s persistent/intermittent.

## Verify Backups

Create/verify a backup for all components if possible (DTR, UCP, Swarm, NFS):

* [https://docs.docker.com/datacenter/dtr/2.3/guides/admin/backups-and-disaster-recovery/](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/backups-and-disaster-recovery/ "https://docs.docker.com/datacenter/dtr/2.3/guides/admin/backups-and-disaster-recovery/")
* [https://docs.docker.com/datacenter/ucp/2.2/reference/cli/backup/](https://docs.docker.com/datacenter/ucp/2.2/reference/cli/backup/ "https://docs.docker.com/datacenter/ucp/2.2/reference/cli/backup/")
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

This matrix describes the relationship between features of Docker Enterprise DTR 2.3.x; this is not exhaustive of all services but rather a starting point for analysis and investigation.

|    | api | registry | rethink | garant | nginx | jobrunner | postgres | notary-server | notary-signer | storage| ucp | 
|---|---|---|---|---|---|---|---|---|---|---|---|
| Docker API |   |   |   |   |   |   |   |   |   |   | ![](./images/thumbs_up_green.png) |
| Docker Login |   |   | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_green.png) |   |   |   |   |   | ![](./images/thumbs_up_green.png) |
| Web Login | ![](./images/thumbs_up_green.png) |   | ![](./images/thumbs_up_yellow.png) |   | ![](./images/thumbs_up_green.png) |   |   |   |   |   | ![](./images/thumbs_up_green.png) |
| Orgs/Teams/Users |   |   |   |   |   |   |   |   |   |   | ![](./images/thumbs_up_green.png) |
| DTR Availability | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_green.png) |   | ![](./images/thumbs_up_green.png) |   |   |   |   |   | ![](./images/thumbs_up_green.png) |
| General DTR API | ![](./images/thumbs_up_green.png) |   | ![](./images/thumbs_up_yellow.png) |   | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_yellow.png) |   |   |   |   |   |
| Permissions | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_yellow.png) |   | ![](./images/thumbs_up_yellow.png) |   |   |   |   |   |   |
| Push/Pull |   | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_yellow.png) |   |   |   |   | ![](./images/thumbs_up_green.png) |   |
| Image Promotion | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_yellow.png) |   | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_green.png) |   |   |   |   |   |
| Image Scanning | ![](./images/thumbs_up_green.png) |   | ![](./images/thumbs_up_yellow.png) |   | ![](./images/thumbs_up_yellow.png) | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_green.png) |   |   |   |   |
| Image Signing | ![](./images/thumbs_up_yellow.png) |   | ![](./images/thumbs_up_yellow.png) |   | ![](./images/thumbs_up_yellow.png) |   |   | ![](./images/thumbs_up_green.png) | ![](./images/thumbs_up_green.png) |   |   |
| Image GC |   |   | ![](./images/thumbs_up_yellow.png) |   |   | ![](./images/thumbs_up_green.png) |   |   |   | ![](./images/thumbs_up_green.png) |   |

## More Troubleshooting

* [https://docs.docker.com/datacenter/dtr/2.3/guides/admin/monitor-and-troubleshoot/troubleshoot-with-logs](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/monitor-and-troubleshoot/troubleshoot-with-logs "https://docs.docker.com/datacenter/dtr/2.3/guides/admin/monitor-and-troubleshoot/troubleshoot-with-logs")
* [https://docs.docker.com/datacenter/dtr/2.3/guides/admin/monitor-and-troubleshoot/](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/monitor-and-troubleshoot/ "https://docs.docker.com/datacenter/dtr/2.3/guides/admin/monitor-and-troubleshoot/")

## Review Logs for Errors

Review DTR logs based upon the above matrix for errors and hints to resolution.

## Resolution

Capture a support dump after every action taken to fix the problem. Avoid taking any actions unless you fully understand what the problem is. If the first set of actions does not fix the problem, avoid taking further actions in the same direction, as the issue might have mutated by the action, and the environment may be surfacing a different problem entirely. Resolve the issues uncovered from the logs. If unsure, find assistance from [docs.docker.com](http://docs.docker.com "http://docs.docker.com") or [kbase](https://success.docker.com/ "https://success.docker.com") to move forward, collect the data captured during the process, and open a case with [Docker Support](https://support.docker.com "https://support.docker.com").

> Pro Tip: In the case where one of the DTR replicas has had containers accidently removed running the dtr/reconfigure command can resolve the problem: [https://docs.docker.com/datacenter/dtr/2.3/reference/cli/reconfigure/#description](https://docs.docker.com/datacenter/dtr/2.3/reference/cli/reconfigure/#description "https://docs.docker.com/datacenter/dtr/2.3/reference/cli/reconfigure/#description").
