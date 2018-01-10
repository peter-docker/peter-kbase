---
type: kbase
version: 3
title: "Why can’t I push images to Docker Hub after logging in?"
summary: "After logging in, if you receive the following error when trying to push images to Docker Hub, you have some issues with the authentication payload: Take note that the authentication endpoint should be https://index.docker.io/v1/. The remaining payload (auth and email) will vary from user to user. In addition, please ensure that you are pushing the images with the following syntax:"
dateCreated: "Wed, 20 Apr 2016 07:01:51 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 61
internal: no
author: "kenny.lim"
platform:
testedon:
tags:
product:
  - Hub
---

After logging in, if you receive the following error when trying to push images to Docker Hub, you have some issues with the authentication payload:

    unauthorized: authentication required

Perform a simple check on the `~/.docker/config.json` file and ensure that the endpoint is correct. The correct configuration should look something like this:

    {
       "auths": {
               "<^>https://index.docker.io/v1/</^^>": {
                       "auth": "Y2hlcnlscTI2OkNoZXJRMjYqKg==",
                       "email": "example@example.com"
               }
       }}

Take note that the authentication endpoint should be `[https://index.docker.io/v1/](https://index.docker.io/v1/ "https://index.docker.io/v1/")`. The remaining payload (`**auth**` and `**email**`) will vary from user to user.

In addition, please ensure that you are pushing the images with the following syntax:

    docker push <^><namespace>/<image>:<tag><^^>

For example:

    docker push <^>mydockerid/hello-world:latest<^^>

Note: This issue commonly arises when you are using Docker Engines that are not from [https://docs.docker.com/engine/installation/](https://docs.docker.com/engine/installation/ "https://docs.docker.com/engine/installation/").
