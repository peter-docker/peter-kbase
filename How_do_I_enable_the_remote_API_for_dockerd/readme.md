---
type: kbase
version: 3
title: "How do I enable the remote API for dockerd"
summary: "The best way to include the required startup options without editing the systemd unit file in place is to use a systemd drop-in file. After completing these steps, you will have enabled the remote API for dockerd, without editing the systemd unit file in place: Note: The -H flag binds dockerd to a listening socket, either a Unix socket or a TCP port. Ensure that anyone that has access to the TCP listening socket is a trusted user since access to the docker daemon is root-equivalent."
dateCreated: "Fri, 15 Sep 2017 15:23:01 GMT"
dateModified: "Wed, 04 Oct 2017 03:32:03 GMT"
dateModified_unix: 1507087923
upvote: 0
internal: no
author: "scasey"
platform:
  - linux
testedon:
tags:
product:
  - EE
---

## Overview

The best way to include the required startup options without editing the systemd unit file in place is to use a systemd drop-in file.

## Resolution

After completing these steps, you will have enabled the remote API for dockerd, without editing the systemd unit file in place:

1. Create a file at `/etc/systemd/system/docker.service.d/startup_options.conf` with the below contents: 
    
        # /etc/systemd/system/docker.service.d/override.conf
        [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2376
    
    > **Note:** The -H flag binds dockerd to a listening socket, either a Unix socket or a TCP port. You can specify multiple -H flags to bind to multiple sockets/ports. The default -H fd:// uses systemd's socket activation feature to refer to /lib/systemd/system/docker.socket.
2. Reload the unit files: 
    
        $ sudo systemctl daemon-reload
3. Restart the docker daemon with new startup options: 
    
        $ sudo systemctl restart docker.service
4. Ensure that anyone that has access to the TCP listening socket is a trusted user since access to the docker daemon is root-equivalent.

# Additional Documentation

* [https://docs.docker.com/engine/security/](https://docs.docker.com/engine/security/ "https://docs.docker.com/engine/security/")