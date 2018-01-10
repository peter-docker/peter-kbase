---
type: kbase
version: 3
title: "How do I backup my Docker container volumes in Docker Cloud to AWS S3?"
summary: "Question How do I backup my Docker container volumes in Docker Cloud to AWS S3? Answer Use the dockercloud/dockup utility image for this purpose. You only need to run it taking the volumes of the container you want to backup with volumes-from and pass it the environment configuration of the container. You can find more info in its Github repo."
dateCreated: "Thu, 08 Oct 2015 19:37:15 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 4
internal: no
author: "justinnevill"
platform:
testedon:
tags:
  - aws
  - backup
  - Docker Cloud
product:
  - Hub
---

## Question

How do I backup my Docker container volumes in Docker Cloud to AWS S3?

## Answer

Use the [dockercloud/dockup](https://hub.docker.com/r/dockercloud/dockup/) utility image for this purpose. You only need to run it taking the volumes of the container you want to backup with `volumes-from` and pass it the environment configuration of the container. You can find more info in its Github repo.
