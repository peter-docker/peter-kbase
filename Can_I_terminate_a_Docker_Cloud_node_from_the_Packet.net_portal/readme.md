---
type: kbase
version: 3
title: "Can I terminate a Docker Cloud node from the Packet.net portal?"
summary: "Can I terminate a Docker Cloud node from the Packet.net portal? If you created the node using Docker Cloud and you terminate it from the portal, all data in that node will be destroyed. If you turn off the device, Docker Cloud marks it asUnreachable because the node has not been terminated but Cloud cannot contact it. If you created the host yourself, added it to Docker Cloud as a “Bring Your Own Node” and then terminated it, the node will be marked as Unreachable until you manually remove it."
dateCreated: "Thu, 08 Oct 2015 19:16:39 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 1
internal: no
author: "justinnevill"
platform:
testedon:
tags:
  - Docker Cloud
product:
  - Hub
uniqueid: KB000199
---
TESTING TESTING TESTING AND MORE TESTING!!!

## Question

Can I terminate a Docker Cloud node from the Packet.net portal?

## Answer

If you created the node using Docker Cloud and you terminate it from the portal, all data in that node will be destroyed. Docker Cloud detects the termination and marks the node as `Terminated`. If you turn off the device, Docker Cloud marks it as`Unreachable` because the node has not been terminated but Cloud cannot contact it.

If you created the host yourself, added it to Docker Cloud as a “Bring Your Own Node” and then terminated it, the node will be marked as `Unreachable` until you manually remove it.
