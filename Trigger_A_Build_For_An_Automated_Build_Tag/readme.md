---
type: kbase
version: 2
title: "Trigger A Build For An Automated Build Tag"
summary: "Question How do you trigger a build for an existing tag? Answer Currently the only way to trigger a rebuild for a tag on Docker Hub is: Remove the tag in Git Push the repository Add the tag back to Git Push the repository again Note: there is an open feature request for this at https://github.com/docker/hub-feedback/issues/620"
dateCreated: "Fri, 25 Mar 2016 17:21:18 GMT"
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

## Question

How do you trigger a build for an existing tag?

## Answer

Currently the only way to trigger a rebuild for a tag on Docker Hub is:

1. Remove the tag in Git
2. Push the repository
3. Add the tag back to Git
4. Push the repository again

Note: there is an open feature request for this at <https://github.com/docker/hub-feedback/issues/620>