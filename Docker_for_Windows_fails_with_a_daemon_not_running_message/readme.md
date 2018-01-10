---
type: kbase
version: 3
title: "Docker for Windows fails with a daemon not running message"
summary: "Docker daemon is not reachable"
dateCreated: "Tue, 25 Jul 2017 15:31:10 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "jasonbivins"
platform:
  - windows
testedon:
tags:
  - daemon
  - error
  - networking
product:
  - D4W
---

## Issue

If the Docker daemon fails to start up with Windows, or stops for some reason, you can get network connection errors or warnings regarding the Docker daemon service when you try to run any commands.

    error during connect: Get http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.30/info: open //./pipe/docker_engine: The system cannot find the file specified. In the default daemon configuration on Windows, 
    the docker client must be run elevated to connect. This error may also indicate that the docker daemon is not running.

## Prerequisites

Before performing these steps, you must meet the following requirements:

* Windows 10
* Docker for Windows

## Resolution

When the Docker daemon fails to start with Windows, or stops unexpectedly during normal operations, you'll get a network connection error regarding the Docker daemon letting you know that it is unreachable. The Docker daemon is also commonly referred to as the Docker for Windows service. Usually, restarting the service will resolve the error.

**Option 1:** Restart the Docker for Windows service through the Windows GUI by right clicking the service and choosing restart:

![](./images/services.png)

    Net stop com.docker.service
    Net start com.docker.service

## What's Next

If this does not resolve your issue, Please file an issue here: [https://github.com/docker/for-win/issues](https://github.com/docker/for-win/issues "https://github.com/docker/for-win/issues")
