---
type: kbase
version: 9
title: "How do I enable 'debug' logging of the Docker daemon?"
summary: "To enable debug logging of the Docker daemon use the following the steps: Send a SIGHUP signal to the Docker daemon, triggering it to reload its configuration without restarting the process. 3. (optional) To check that the configuration has been applied, check docker info. If debug is enabled, the output will show Debug Mode (server): true. To disable debug logging, perform both steps again, setting 'debug': false. Refer to the Compatibility Matrix for a list of specific versions."
dateCreated: "Tue, 17 May 2016 20:06:07 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 13
internal: no
author: "nhazlett"
platform:
  - linux
testedon:
tags:
- daemon
- logging
product:
  - EE
---

To enable debug logging of the Docker daemon use the following the steps:

1. Edit `/etc/docker/daemon.json`, setting`"debug": true`. If this file does not exist, it should look like this when complete:

    {
        "debug": <^>true<^^>
    }

2. Send a SIGHUP signal to the Docker daemon, triggering it to reload its configuration without restarting the process.

    sudo kill -SIGHUP $(pidof dockerd)

3. (optional) To check that the configuration has been applied, check `docker info`. If debug is enabled, the output will show `Debug Mode (server): true`.

    docker info | grep -i debug.*server



To disable debug logging, perform both steps again, setting `"debug": <^>false<^^>`.

This process works on all supported versions of Linux. Refer to the [Compatibility Matrix](https://success.docker.com/Policies/Compatibility_Matrix "Compatibility Matrix") for a list of specific versions.

The product documentation for this procedure can be found here: [https://docs.docker.com/engine/admin...able-debugging](https://docs.docker.com/engine/admin/#enable-debugging "https://docs.docker.com/engine/admin/#enable-debugging")
