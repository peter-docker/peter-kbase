---
type: kbase
version: 3
title: "I have an automated build running on Docker Cloud or Docker Hub. How do I manually stop the builds?"
summary: "There is currently no way to stop a running build. On Docker Cloud, the image building process will automatically timeout and stop after 4 hours regardless of its state. On Docker Hub, builds will timeout and stop after 2 hours. Note: Builds that time out will end up in failed state."
dateCreated: "Tue, 29 Mar 2016 05:51:55 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "kenny.lim"
platform:
testedon:
tags:
product:
  - Hub
---

There is currently no way to stop a running build.

On Docker Cloud, the image building process will automatically timeout and stop after 4 hours regardless of its state. On Docker Hub, builds will timeout and stop after 2 hours. **Note**: Builds that time out will end up in failed state.