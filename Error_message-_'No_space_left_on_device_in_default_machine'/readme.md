---
type: kbase
version: 3
title: "Error message: 'No space left on device in default machine'"
summary: "When I try to start a container I get a No space left on device in default machine error."
dateCreated: "Thu, 10 Mar 2016 23:40:59 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 1
internal: no
author: "justinnevill"
platform:
testedon:
tags:
product:
  - EE
---

### Overview

When I try to start a container I get a `No space left on device in default machine` error.

### Diagnostic Steps

Here are a few commands that can help diagnose what's going on:

    docker-machine ssh default
    $ sudo find /var/lib/docker/ -maxdepth 1 -mindepth 1 | xargs sudo du -sch

This should give you an idea as to what is taking up space, for example:

    4.0K    /var/lib/docker/repositories-aufs
    56.0M   /var/lib/docker/graph
    8.0K    /var/lib/docker/trust
    14.7M   /var/lib/docker/init
    104.0K  /var/lib/docker/.migration-v1-images.json
    6.1G    /var/lib/docker/aufs
    156.0K  /var/lib/docker/containers
    24.3M   /var/lib/docker/image
    272.9M  /var/lib/docker/tmp
    1.6G    /var/lib/docker/volumes
    108.0K  /var/lib/docker/network
    152.0K  /var/lib/docker/linkgraph.db
    230.5M  /var/lib/docker/vfs
    0   /var/lib/docker/.migration-v1-tags
    8.3G    total

### Resolution

To clean up images, try removing all images that aren't currently in use:

    $ docker images -aq -f 'dangling=true' | xargs docker rmi

You may still have volumes to worry about. To clean those up:

    $ docker volume ls -q -f 'dangling=true' | xargs docker volume rm

**WARNING:** Take care with this command, as it will remove all dangling volumes whether they are anonymous or named.