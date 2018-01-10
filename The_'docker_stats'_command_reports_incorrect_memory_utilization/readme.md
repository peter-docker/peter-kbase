---
type: kbase
version: 3
title: "The 'docker stats' command reports incorrect memory utilization"
summary: "As of Docker EE 17.03, the Docker daemon stats endpoint incorrectly reports memory utilization as the sum of rss and the page cache allocated to the container. A backport of the upstream fix (https://github.com/docker/cli/pull/80) for this issue is being tracked by internal engineering issue orca/7241. Release of the fix for Docker Enterprise Edition is currently targeted for release with version 17.06. The Linux kernel interface consumed by docker stats can be found under:"
dateCreated: "Mon, 17 Jul 2017 20:30:06 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "trapier"
platform:
  - linux
testedon:
tags:
product:
  - EE
---

As of Docker EE 17.03, the Docker daemon `stats` endpoint incorrectly reports memory utilization as the *sum* of rss and the page cache allocated to the container.

A backport of the upstream fix ([https://github.com/docker/cli/pull/80](https://github.com/docker/cli/pull/80 "https://github.com/docker/cli/pull/80")) for this issue is being tracked by internal engineering issue `orca/7241`. Release of the fix for Docker Enterprise Edition is currently targeted for release with version 17.06. A backport to version 17.03 is also in progress.

The Linux kernel interface consumed by `docker stats` can be found under:

    /sys/fs/cgroup/memory/docker/<^>6b073e1c654af637b2337be7bd86d464502f0aa42f98c6f2c6ac83e4b5fe177<^^>/memory.stat

Further documentation for this kernel interface can be found here:

* [https://docs.docker.com/engine/admin/runmetrics/#memory-metrics-memorystat](https://docs.docker.com/engine/admin/runmetrics/#memory-metrics-memorystat "https://docs.docker.com/engine/admin/runmetrics/#memory-metrics-memorystat")
