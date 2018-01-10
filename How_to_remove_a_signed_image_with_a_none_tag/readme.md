---
type: kbase
title: "How to remove a signed image with a <none> tag"
dateCreated: "Thu, 24 Dec 2015 20:29:30 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
internal: no
author: "pvnovarese"
platform:
testedon:
tags:
  - Docker Content Trust
  - daemon
product:
  - EE
---

### Overview

When using Docker Content Trust, after pulling a signed image, there are two entries in **docker images** output - one with the tag you requested, and one with a **<none>** tag. This is normal output. There is only one image instance. However, when removing the image, both entries must be deleted in the proper order.

For example:

    # **export DOCKER_CONTENT_TRUST=1**
    # **docker pull pvnovarese/mprime:latest**
    Pull (1 of 1): pvnovarese/mprime:latest@sha256:0b315a681a6b9f14f93ab34f3c744fd547bda30a03b55263d93861671fa33b00
    sha256:0b315a681a6b9f14f93ab34f3c744fd547bda30a03b55263d93861671fa33b00: Pulling from pvnovarese/mprime
    a3ed95caeb02: Pull complete
    546e579918ed: Pull complete
    Digest: sha256:0b315a681a6b9f14f93ab34f3c744fd547bda30a03b55263d93861671fa33b00
    Status: Downloaded newer image for pvnovarese/mprime@sha256:0b315a681a6b9f14f93ab34f3c744fd547bda30a03b55263d93861671fa33b00
    Tagging pvnovarese/mprime@sha256:0b315a681a6b9f14f93ab34f3c744fd547bda30a03b55263d93861671fa33b00 as pvnovarese/mprime:latest
    # **docker images**
    REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
    pvnovarese/mprime        latest              459769dbc7a1        5 days ago          4.461 MB
    pvnovarese/mprime        <none>              459769dbc7a1        5 days ago          4.461 MB

### Diagnostic Steps

You can see the difference in these two entries if you use the 
**--digests=true** option (the untagged entry has the Docker Content Trust signature digest):

    # **docker images --digests=true**
    REPOSITORY               TAG                 DIGEST                                                                    IMAGE ID            CREATED             SIZE
    pvnovarese/mprime        latest              <none>                                                                    459769dbc7a1        5 days ago          4.461 MB
    pvnovarese/mprime        <none>              sha256:0b315a681a6b9f14f93ab34f3c744fd547bda30a03b55263d93861671fa33b00   459769dbc7a1        5 days ago          4.461 MB

You can create new containers by specifying either the tag or the digest:

    # **docker run -d pvnovarese/mprime:latest**
    001df1fcc808ac490b60dbd94805433282947544d95807340e0d7a9da52e7138
    # **docker run -d pvnovarese/mprime@sha256:0b315a681a6b9f14f93ab34f3c744fd547bda30a03b55263d93861671fa33b00**
    f83ba1e30327d34d0e05cf9d94055d69adc1bda74681b06fd8ab7f708d303f5a

> **Note:** in this case, specifying the tag will induce the Engine to verify the trust data before creating the container. If only the digest is specified, then the Engine will trust that particular signature digest and immediately start the container. You can see the details by using the **-D** flag to enable debug output, e.g.:

    # **docker -D run -d pvnovarese/mprime:latest**
    DEBU[0000] reading certificate directory: /home/pvn/.docker/tls/notary.docker.io
       [... debug output snipped for brevity ...]
    DEBU[0001] successfully verified targets
    33e8b590c835371b43a2c32edb47cc08538097b3b35891460176052dbee05f34

compared to:

    # **docker -D run -d pvnovarese/mprime@sha256:0b315a681a6b9f14f93ab34f3c744fd547bda30a03b55263d93861671fa33b00**
    17e921adb660e621a1eb05a5e291a86a538fef4bbe20f43d54f73a1e5117dd30

### Removing the Signed Image

If you would like to remove the signed image, the easiest method is to first remove the tagged entry, then remove the image by specifying its Image ID:

    # **docker images**
    REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
    pvnovarese/mprime        latest              459769dbc7a1        5 days ago          4.461 MB
    pvnovarese/mprime        <none>              459769dbc7a1        5 days ago          4.461 MB
    # **docker rmi pvnovarese/mprime:latest**
    Untagged: pvnovarese/mprime:latest
    # **docker rmi 459769dbc7a1**
    Deleted: sha256:459769dbc7a1cfb3a866102c0a527b1fb0b09b8b3a827f3f122dfd16b9fafb08
    Deleted: sha256:f73e8f96d63c018dc0669de123a80bf44986dc3451b09c2a14b9acc72a336ab7
    Deleted: sha256:5126a1af5d4c6e57a2f4b7aa10edf1628920132ef1dff9dc0032ba3653d413a2
    Deleted: sha256:2d5b0fcd6d0995b0f0b796decf69ebc4bf8192b50a7fe3796b46fa9ac1c06654

> **Note:** This behavior might change in future releases. Check out this GitHub issue about improving the user experience: [https://github.com/docker/docker/issues/18892](https://github.com/docker/docker/issues/18892 https://github.com/docker/docker/issues/18892).
