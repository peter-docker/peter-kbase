---
type: kbase
version: 2
title: "Abort signal on Docker host putty session during Docker Engine restart"
summary: "When using Putty with Linux hosts on VMWare, abort signals can occur when the Putty keepalive is too low. In the Putty configuration window, change the Connection setting Seconds between keepalive to 5."
dateCreated: "Tue, 08 Aug 2017 14:53:55 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 0
internal: no
author: "klamb"
platform:
testedon:
tags:
product:
  - EE
uniqueid: KB000185
---

TESTING TESTING TESTING AND MORE TESTING!!!

When using Putty with Linux hosts on VMWare, abort signals can occur when the Putty keepalive is too low.  
  
In the Putty configuration window, change the Connection setting `Seconds between keepalive` to `5`.
