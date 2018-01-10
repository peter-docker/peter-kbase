---
type: kbase
version: 4
title: "Why are memory metrics not being reported?"
summary: "In order to report accurate memory metrics, Docker Engine needs certain kernel settings to be enabled. Ubuntu and Debian are commonly missing these settings; you can learn how to enable them in this installation documentation."
dateCreated: "Fri, 10 Feb 2017 14:21:41 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "justinnevill"
platform:
  - linux
testedon:
tags:
product:
  - EE
---

In order to report accurate memory metrics, Docker Engine needs certain kernel settings to be enabled. Ubuntu and Debian are commonly missing these settings; you can learn how to enable them in this [installation documentation](https://docs.docker.com/v1.12/engine/installation/linux/ubuntulinux/#/enable-memory-and-swap-accounting "https://docs.docker.com/v1.12/engine/installation/linux/ubuntulinux/#/enable-memory-and-swap-accounting").