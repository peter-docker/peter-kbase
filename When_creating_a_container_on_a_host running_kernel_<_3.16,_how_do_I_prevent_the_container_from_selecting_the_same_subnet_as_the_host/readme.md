---
type: kbase
version: 5
title: "When creating a container on a host running kernel < 3.16, how do I prevent the container from selecting the same subnet as the host?"
summary: "When creating a container on a host running kernel < 3.16, if the network has a subnet which overlaps with the host, you may receive the following error: This only occurs when the kernel is < 3.16 AND the network has a subnet which overlaps with the host. To resolve this issue, use the --subnet option with the network create command and enable the --ip feature for docker run."
dateCreated: "Mon, 19 Dec 2016 15:42:14 GMT"
dateModified: "Fri, 10 Mar 2017 16:16:01 GMT"
dateModified_unix: 1489162561
upvote: 0
internal: yes
author: "scasey"
platform:
  - linux
testedon:
tags:
product:
  - EE
---

When creating a container on a host running kernel < 3.16, if the network has a subnet which overlaps with the host, you may receive the following error:

    docker: Error response from daemon: 409 Conflict: subnet sandbox join
    failed for "
    10.0.2.0/24": overlay subnet 
    10.0.2.0/24 has conflicts in the
    host while running in host mode.

This only occurs when the kernel is < 3.16 AND the network has a subnet which overlaps with the host. Overlay network is a global option.

To resolve this issue, use the `--subnet` option with the `network create` command and enable the `--ip` feature for `docker run`. If a network is created without the `--subnet` option, then the `--ip` feature in `docker run` will not work.