---
type: kbase
version: 2
title: "Why does auto deploy not work when I use a new tag on a Docker Hub image?"
summary: "Currently, the auto-deploy feature only triggers when you push images with the latest tag. If you push other tags, it will not trigger an automated re-deploy for the service. You will need to manually stop/start the service again."
dateCreated: "Thu, 19 May 2016 11:47:48 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 1
internal: no
author: "kizbitz"
platform:
testedon:
tags:
product:
  - Hub
---

Currently, the auto-deploy feature only triggers when you push images with the `latest` tag. If you push other tags, it will not trigger an automated re-deploy for the service.

You will need to manually stop/start the service again.