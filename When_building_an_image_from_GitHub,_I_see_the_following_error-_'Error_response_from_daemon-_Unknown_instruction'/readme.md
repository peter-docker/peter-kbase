---
type: kbase
version: 3
title: "When building an image from GitHub, I see the following error: 'Error response from daemon: Unknown instruction'"
summary: "When attempting to build an image using a source repository from GitHub, the build fails with the following error: Error response from daemon: Unknown instruction If the build is failing for a sample command such as the following: docker build https://github.com/user_me/my_repo Add .git to the end of the command to build without the Unknown instruction error: docker build https://github.com/user_me/my_repo.git"
dateCreated: "Mon, 08 May 2017 22:26:48 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: bryceryan-docker
platform:
testedon:
tags:
  - error
  - daemon
product:
  - EE
---

When attempting to build an image using a source repository from GitHub, the build fails with the following error:

    Error response from daemon: Unknown instruction

If the build is failing for a sample command such as the following:

    docker build https://github.com/user_me/my_repo

Add `.git` to the end of the command to build without the `Unknown instruction` error:

    docker build https://github.com/user_me/my_repo<^>.git<^^>
