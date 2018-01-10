---
type: kbase
version: 3
title: "Is it possible to increase the number of nodes to more than 10 on Docker Cloud?"
summary: "Is it possible to increase the number of nodes to more than 10 on Docker Cloud? Is it possible to increase the size of nodes in an existing cluster on Docker Cloud? (e.g. It is possible to increase the maximum number of nodes on Docker Cloud. As for increasing the size of nodes in an existing cluster, the only method to achieve that currently is to rebuild the cluster as the node size can only be adjusted during cluster creation."
dateCreated: "Fri, 22 Apr 2016 03:49:27 GMT"
dateModified: "Sat, 15 Oct 2016 21:00:02 GMT"
dateModified_unix: 1476565202
upvote: 0
internal: no
author: "kenny.lim"
platform:
testedon:
tags:
product:
---

## Question

* Is it possible to increase the number of nodes to more than 10 on Docker Cloud?
* Is it possible to increase the size of nodes in an existing cluster on Docker Cloud? (e.g. from a t2.small to m4.large)

## Answer

It is possible to increase the maximum number of nodes on Docker Cloud. However, it is not possible to increase the number of nodes per cluster currently.

All the quotas on your account page ([https://cloud.docker.com/account/#quota](https://cloud.docker.com/account/#quota "https://cloud.docker.com/account/#quota")) are qualified for adjustment by [opening a support case](https://support.docker.com/ "https://support.docker.com/").

As for increasing the size of nodes in an existing cluster, the only method to achieve that currently is to rebuild the cluster as the node size can only be adjusted during cluster creation.