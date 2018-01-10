---
type: kbase
version: 4
title: "When using a VM template to add a node to UCP, why does the status show pending?"
summary: "When using a VM template to add a node to UCP, the status of the node in the UCP UI shows the node in a pending state. When accessing the dashboard, nodes may disappear as well. To successfully add the node to the cluster, check that the VM template doesn't include the /etc/docker/key.json file. If it is, remove it, and restart the Docker daemon. The key.json is used as the id of the node, and having multiple nodes with the same id will produce dashboard symptoms."
dateCreated: "Wed, 25 Jan 2017 20:41:17 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
  - linux
testedon:
tags:
product:
  - EE
---

When using a VM template to add a node to UCP, the status of the node in the UCP UI shows the node in a pending state. When accessing the dashboard, nodes may disappear as well.

To successfully add the node to the cluster, check that the VM template doesn't include the `/etc/docker/key.json` file. If it is, remove it, and restart the Docker daemon. The `key.json` is used as the id of the node, and having multiple nodes with the same id will produce dashboard symptoms.