---
type: kbase
version: 3
title: "When a created container fails, how long will it be kept?"
summary: "Docker's garbage collection (GC) processes will not remove any failed container (including volumes and networks). These are intentionally left so that they may be inspected and so that any issues can be remediated. Removal of these elements is manual."
dateCreated: "Thu, 06 Jul 2017 13:02:54 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "klamb"
platform:
testedon:
tags:
product:
---

Docker's garbage collection (GC) processes will not remove any failed container (including volumes and networks). These are intentionally left so that they may be inspected and so that any issues can be remediated.  
  
Removal of these elements is manual.