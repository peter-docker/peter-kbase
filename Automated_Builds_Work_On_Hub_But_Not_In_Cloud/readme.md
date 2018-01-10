---
type: kbase
version: 2
title: "Automated Builds Work On Hub But Not In Cloud"
summary: "If your automated builds complete successfully on Docker Hub but not in Docker Cloud, the cause is usually one of the following: You're using BYON for a build node and have no nodes available. A problem with a BYON. BYON builds are different because the builds run in DinD. Try changing the autobuild configuration to 'Build in Docker Cloud Infrastructure.' The build should be 100% compatible in Docker Cloud/Docker Hub."
dateCreated: "Thu, 15 Sep 2016 20:09:00 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 1
internal: no
author: "kizbitz"
platform:
testedon:
tags:
product:
  - Hub
uniqueid: KB000190
---

TESTING TESTING TESTING AND MORE TESTING!!!

If your automated builds complete successfully on Docker Hub but not in Docker Cloud, the cause is usually one of the following:

* You're using BYON for a build node and have no nodes available.
* A problem with a BYON. BYON builds are different because the builds run in DinD. Try changing the autobuild configuration to "Build in Docker Cloud Infrastructure." The build should be 100% compatible in Docker Cloud/Docker Hub.