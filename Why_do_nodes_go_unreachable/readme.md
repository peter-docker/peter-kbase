---
type: kbase
version: 2
title: "Why do nodes go unreachable?"
summary: "Why do nodes go unreachable? Nodes often go 'unreachable' when they run out of resources (memory or CPU) and Docker Cloud cannot contact them. This can be because too many containers are deployed, or the ones deployed are taking up all resources. If this is the case, please upgrade your nodes to a newer version. If you keep seeing this behavior and you are confident it's not being caused by your containers, please let us know!"
dateCreated: "Tue, 06 Oct 2015 17:02:24 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 15
internal: no
author: "justinnevill"
platform:
testedon:
tags:
  - Docker Cloud
product:
  - Hub
---

## Question

Why do nodes go unreachable?

## Answer

Nodes often go "unreachable" when they run out of resources (memory or CPU) and Docker Cloud cannot contact them. This can be because too many containers are deployed, or the ones deployed are taking up all resources.

It can also be due to a Docker memory leak that affects Docker versions 1.6 and earlier. If this is the case, please upgrade your nodes to a newer version.

If you have access to the node, a simple restart of the node will help mitigate.

If you keep seeing this behavior and you are confident it's not being caused by your containers, please let us know!
