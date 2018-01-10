---
type: kbase
version: 6
title: "Upgrading from Docker CE to Docker EE"
summary: "Docker Community Edition (CE) is ideal for developers and small teams looking to get started with Docker and experimenting with container-based apps. Since CE and EE are distributed as separate application binaries, the migration path from CE to EE involves reinstalling Docker and cannot be performed in an existing environment without redeploying the environment."
dateCreated: "Fri, 05 May 2017 16:10:53 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "trapier"
platform:
testedon:
tags:
  - upgrading
product:
  - EE
---

Docker Community Edition (CE) is ideal for developers and small teams looking to get started with Docker and experimenting with container-based apps. Docker Enterprise Edition (EE) is designed for enterprise development and IT teams who build, ship, and run business critical applications in production at scale. Users who start off on CE may wish to change to EE for business operational or other reasons.

Since CE and EE are distributed as separate application binaries, the migration path from CE to EE involves reinstalling Docker and cannot be performed in an existing environment without redeploying the environment.

Uninstall Docker CE, and then install Docker EE.
