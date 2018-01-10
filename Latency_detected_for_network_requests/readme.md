---
type: kbase
version: 2
title: "Latency detected for network requests"
summary: "This means that it's taking more than 2 seconds for two UCP manager nodes to communicate with one another, which can cause reliability problems for your UCP cluster. For example, this can cause internal components to think other nodes are down and trigger unnecessary leadership elections, resulting in temporary outages and reduced performance. If you're experiencing this, you should decrease the latency of the network the UCP nodes use to communicate."
dateCreated: "Tue, 09 May 2017 21:54:37 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "tfoxnc"
platform:
testedon:
tags:
 - networking
 - error
product:
  - EE
---

If UCP is displaying a warning banner with the following message:

    network requests to 
    <node> are taking 
    <value> seconds

This means that it's taking more than `2` seconds for two UCP manager nodes to communicate with one another, which can cause reliability problems for your UCP cluster. For example, this can cause internal components to think other nodes are down and trigger unnecessary leadership elections, resulting in temporary outages and reduced performance.

If you're experiencing this, you should decrease the latency of the network the UCP nodes use to communicate.

[Learn more about the maximum network latency for each UCP component](https://docs.docker.com/datacenter/ucp/2.1/guides/admin/install/system-requirements/).
