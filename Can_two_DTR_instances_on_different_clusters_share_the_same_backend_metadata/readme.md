---
type: kbase
version: 2
title: "Can two DTR instances on different clusters share the same backend metadata?"
summary: "No, it is not possible to have two separate DTR instances sharing the same backend metadata. If you want registry information replicated across multiple DTR hosts with shared backend metadata, they need to be replicas of one another in the same cluster. Using two separate DTR instances which are sharing the same backend (e.g. The proper way to configure multiple DTR hosts is to set them up as replicas in a cluster to ensure they stay in sync."
dateCreated: "Wed, 22 Feb 2017 21:41:56 GMT"
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

No, it is not possible to have two separate DTR instances sharing the same backend metadata. If you want registry information replicated across multiple DTR hosts with shared backend metadata, they need to be replicas of one another in the same cluster. Using two separate DTR instances which are sharing the same backend (e.g. mounted via network volume) is very likely to cause some corruption at some point. The proper way to configure multiple DTR hosts is to set them up as replicas in a cluster to ensure they stay in sync.