---
type: kbase
version: 2
title: "What happens if I restart a node in the Packet.net portal?"
summary: "Question What happens if I restart a node in the Packet.net portal? Answer After the node boots up, Docker Cloud Agent tries to contact our API and register itself with the new IP. Cloud then automatically updates the DNS of the node and the containers on it to use the new IP. The node changes state from UnreachabletoDeployed."
dateCreated: "Thu, 08 Oct 2015 19:15:34 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
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

What happens if I restart a node in the Packet.net portal?

## Answer

After the node boots up, Docker Cloud Agent tries to contact our API and register itself with the new IP. Cloud then automatically updates the DNS of the node and the containers on it to use the new IP. The node changes state from `Unreachable`to`Deployed`.
