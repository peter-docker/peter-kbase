---
type: kbase
version: 7
title: "Getting 'unauthorized: authentication required' when trying to push image to DTR"
summary: "When trying to push an image to DTR even after successfully logging in, I get: One important thing to note is that with DTR version 1.4.x, you need to create the repositories with UI/API calls before you can actually push to DTR. If you need to push a new image, say my-busybox, then you will first need to create it using Web UI or API calls, under the proper user/organization, and then push the image using:"
dateCreated: "Mon, 09 May 2016 22:33:03 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 28
internal: no
author: "sabin.basyal"
platform:
testedon:
  - dtr-1.4.0
  - dtr-1.4.1
  - dtr-1.4.2
  - dtr-1.4.3
tags:
  - error
product:
  - EE
---

# Overview

When trying to push an image to DTR even after successfully logging in, I get:

    unauthorized: authentication required

# Resolution

It is possible that the way you are trying to push the image is incorrect. One important thing to note is that with DTR version 1.4.x, you need to create the repositories with UI/API calls before you can actually push to DTR.  
  
For example, to push your custom image `test`:

* First go to WebUI and create a repo named `test` under `admin` (i.e. `admin/test`)
* `docker pull busybox`
* `docker tag busybox <^><your-dtr-instance><^^>/admin/test:tag1`
* `docker push <^><your-dtr-instance><^^>/admin/test:tag1`

If you need to push a new image, say **my-busybox**, then you will first need to create it using Web UI or API calls, under the proper user/organization, and then push the image using:

    docker push <^><your-dtr-instance><^^>/<^><user-name><^^>/my-busybox

With DTR 1.4.x and above, the repository must be created from the UI or API first. Creating repositories on push are disallowed for the following reasons:

1. To discourage security leaks where a private image is accidentally pushed to a non-existent repository, thus created as a public repository
2. To prevent the accidental creation of repositories due to a typo
