---
type: kbase
version: 3
title: "Can I terminate a Docker Cloud node from the AWS console?"
summary: "If you created the node using Docker Cloud and you terminate it in the AWS console, all data in that node will be destroyed, as the volume attached to it is set to destroy on node termination. If you haven’t revoked your Docker Cloud IAM user, Cloud detects the termination and marks the node as Terminated. If you created the host yourself, added it to Docker Cloud as a “Bring Your Own Node” and then terminated it, the node will stay Unreachable until you manually remove it."
dateCreated: "Thu, 08 Oct 2015 19:33:47 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 4
internal: no
author: "justinnevill"
platform:
testedon:
tags:
  - aws
  - Docker Cloud
product:
  - Hub
uniqueid: KB000198
---

TESTING TESTING TESTING AND MORE TESTING!!!

## Question

Can I terminate a Docker Cloud node from the AWS console?

## Answer

If you created the node using Docker Cloud and you terminate it in the AWS console, all data in that node will be destroyed, as the volume attached to it is set to destroy on node termination. If you haven’t revoked your Docker Cloud IAM user, Cloud detects the termination and marks the node as `Terminated`.

If you created the host yourself, added it to Docker Cloud as a “Bring Your Own Node” and then terminated it, the node will stay `Unreachable` until you manually remove it.
