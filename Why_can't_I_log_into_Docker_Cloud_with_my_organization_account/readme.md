---
type: kbase
version: 6
title: "Why can't I log into Docker Cloud with my organization account?"
summary: "You cannot directly log into Docker Cloud or Docker Hub with an organization account. You can then deploy repositories from that organization onto your nodes if you have the necessary permissions to access them. If the organization account had nodes and services running on Docker Cloud prior to being converted, you may no longer have access to them. Please open a support request so we can recover your Stackfiles and you can deploy them on new nodes controlled by another user account you own."
dateCreated: "Wed, 23 Mar 2016 15:49:13 GMT"
dateModified: "Tue, 03 Jan 2017 16:18:20 GMT"
dateModified_unix: 1483460300
upvote: 2
internal: no
author: "yiming"
platform:
testedon:
tags:
product:
  - Hub
---

You cannot directly log into Docker Cloud or Docker Hub with an organization account. Instead, login with a user account that is a member of a team in the organization. You can then deploy repositories from that organization onto your nodes if you have the necessary permissions to access them.

If the organization account had nodes and services running on Docker Cloud prior to being converted, you may no longer have access to them. Please [open a support request](https://support.docker.com/ "https://support.docker.com/") so we can recover your Stackfiles and you can deploy them on new nodes controlled by another user account you own.