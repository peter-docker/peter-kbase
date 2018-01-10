---
type: kbase
version: 3
title: "Can multiple UCP clusters share one DTR?"
summary: "Currently, a DTR instance can only be associated with one UCP cluster (one-to-one mapping). Multiple UCP clusters can't share a single DTR instance. This configuration constraint my change in the future. In the meantime, it is possible to pull images from a DTR instance that's associated with one UCP cluster using a worker node in another cluster, however DTR/UCP integration features such as Docker Content Trust are only available to the UCP cluster associated with the DTR instance."
dateCreated: "Wed, 21 Dec 2016 19:25:01 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 0
internal: no
author: "justinnevill"
platform:
testedon:
tags:
product:
  - EE
uniqueid: KB000204
---

TESTING TESTING TESTING AND MORE TESTING!!!

Currently, a DTR instance can only be associated with one UCP cluster (one-to-one mapping). Multiple UCP clusters can't share a single DTR instance.

This configuration constraint my change in the future. In the meantime, it is possible to pull images from a DTR instance that's associated with one UCP cluster using a worker node in another cluster, however DTR/UCP integration features such as Docker Content Trust are only available to the UCP cluster associated with the DTR instance.