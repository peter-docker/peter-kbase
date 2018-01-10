---
type: kbase
version: 2
title: "What happens if I restart a node in the AWS console?"
summary: "Question What happens if I restart a node in the AWS console? Answer After the node boots up, the Docker Cloud Agent tries to contact the Cloud API and register itself with the new IP. Docker Cloud automatically updates the DNS of the node and the containers on it to use the new IP. The node’s state changes fromUnreachableto Deployed."
dateCreated: "Thu, 08 Oct 2015 19:30:54 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "justinnevill"
platform:
testedon:
tags:
  - aws
  - Docker Cloud
product:
  - Hub
---

## Question

What happens if I restart a node in the AWS console?

## Answer

After the node boots up, the Docker Cloud Agent tries to contact the Cloud API and register itself with the new IP. Docker Cloud automatically updates the DNS of the node and the containers on it to use the new IP. The node’s state changes from`Unreachable`to `Deployed`.
