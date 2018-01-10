---
type: kbase
version: 4
title: "Multiple failures in HA cluster can lead to loss of cluster nodes"
summary: "When you join non-replica nodes in a UCP HA cluster, the nodes are configured to talk to that specific cluster for cluster membership advertisement. However, if the non-replica node restarts/reboots while the specific controller it was joined to is still down, the non-replica will fail to talk to that specific controller after rebooting, and the node will drop out of the cluster."
dateCreated: "Tue, 01 Mar 2016 20:35:02 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "kizbitz"
platform:
testedon:
  - ucp-1.0.0
  - ucp-1.0.1
  - ucp-1.0.2
  - ucp-1.0.3
  - ucp-1.0.4
tags:
product:
  - EE
---

**NOTE**: This issue is fixed for UCP 1.1 and higher.

When you join non-replica nodes in a UCP HA cluster, the nodes are configured to talk to that specific cluster for cluster membership advertisement. Through that specific controller node, they will auto-detect the other controllers.

If that specific controller fails, the non-replica node will continue to advertise to other controllers in the cluster and everything will work fine. However, if the non-replica node restarts/reboots while the specific controller it was joined to is still down, the non-replica will fail to talk to that specific controller after rebooting, and the node will drop out of the cluster. Once that specific controller comes back, the node will detect and re-advertise.

This issue was tracked at <https://github.com/docker/orca/issues/670>.