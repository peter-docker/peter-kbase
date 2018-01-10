---
type: kbase
version: 2
title: "UCP is running devicemapper in a mode that isn't recommended for production workloads"
summary: "Docker hosts running the devicemapper storage driver default to a configuration mode known as loop-lvm. This mode uses sparse files to build the thin pool used by image and container snapshots. The mode is designed to work out-of-the-box with no additional configuration. UCP production deployments should not run under loop-lvm mode. The preferred configuration for UCP production deployments is direct-lvm."
dateCreated: "Fri, 11 Nov 2016 13:16:55 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 4
internal: no
author: "justinnevill"
platform:
testedon:
tags:
product:
  - EE
---

Docker hosts running the devicemapper storage driver default to a configuration mode known as `loop-lvm`. This mode uses sparse files to build the thin pool used by image and container snapshots. The mode is designed to work out-of-the-box with no additional configuration. UCP production deployments should not run under `loop-lvm` mode.

The preferred configuration for UCP production deployments is `direct-lvm`. To configure `direct-lvm`, please check the following link: <https://docs.docker.com/engine/userguide/storagedriver/device-mapper-driver/#configure-direct-lvm-mode-for-production>
