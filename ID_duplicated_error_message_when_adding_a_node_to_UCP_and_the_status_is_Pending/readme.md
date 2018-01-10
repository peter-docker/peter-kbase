---
type: kbase
version: 3
title: "ID duplicated error message when adding a node to UCP and the status is Pending"
summary: "When trying to add a node to a cluster, the following error occurs with the status of the node showing pending in the UCP UI: Most likely what has happened is the Engine IDs have been duplicated, prior to joining the UCP Swarm cluster. The Engine ID is generated on Docker's first run and stored in /etc/docker/key.json. When creating a VM template, make sure /etc/docker/key.json is not included in the image."
dateCreated: "Fri, 19 May 2017 14:27:19 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
testedon:
tags:
product:
  - EE
---

## Issue

When trying to add a node to a cluster, the following error occurs with the status of the node showing pending in the UCP UI:

    ID duplicated

## Solution

Most likely what has happened is the Engine IDs have been duplicated, prior to joining the UCP Swarm cluster. In most cases, this is a consequence of cloning VMs with Engine already installed. The Engine ID is generated on Docker's first run and stored in `/etc/docker/key.json`. When creating a VM template, make sure `/etc/docker/key.json` is *not* included in the image.

To recover the existing installation, remove `/etc/docker/key.json` and restart Docker on each node.