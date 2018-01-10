---
type: kbase
version: 5
title: "RethinkDB is having memory issues: Some data placed into swap memory in the past hour"
summary: "Warning: The following replicas are unhealthy: 0ddb411b100d; Reasons: Rethink replica '0ddb411b100d' is having memory issues: Some RethinkDB data on this server has been placed into swap memory in the past hour. When RethinkDB places data into swap, it means your node is reaching the limitations of the memory allocated for it. You can add more physical RAM, increase the RAM available to the node during provisioning, or move to different hardware with more memory."
dateCreated: "Tue, 28 Mar 2017 20:09:21 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "curtisz"
platform:
testedon:
tags:
product:
  - EE
---

When running Docker EE, you may at some point see the following error message:

    Warning: The following replicas are unhealthy: 0ddb411b100d; Reasons: Rethink replica '0ddb411b100d' is having memory issues: Some RethinkDB data on this server has been placed into swap memory in the past hour. This may impact performance.

When RethinkDB places data into swap, it means your node is reaching the limitations of the memory allocated for it. To resolve this issue make more memory available to your node. You can add more physical RAM, increase the RAM available to the node during provisioning, or move to different hardware with more memory.
