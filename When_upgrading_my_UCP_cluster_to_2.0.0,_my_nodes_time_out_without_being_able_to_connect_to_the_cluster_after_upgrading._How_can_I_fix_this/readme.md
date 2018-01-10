---
type: kbase
version: 3
title: "When upgrading my UCP cluster to 2.0.0, my nodes time out without being able to connect to the cluster after upgrading. How can I fix this?"
summary: "UCP 2.0.0 and above use the Docker Swarm functions built into the 1.12 CS Engine and higher, instead of the classic Docker Swarm method in earlier versions. Ensure that incoming port 2377 is available in addition to the existing ports opened for Docker Swarm communication. Your nodes should automatically connect to the controllers after opening the necessary ports, and the required UCP 2.0.0 containers will be deployed by the controllers onto those nodes."
dateCreated: "Thu, 17 Nov 2016 15:32:35 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "yiming"
platform:
testedon:
  - ucp-2.0.0
tags:
  - upgrading
product:
  - EE
---

## Issue

UCP 2.0.0 and above use the Docker Swarm functions built into the 1.12 CS Engine and higher, instead of the classic Docker Swarm method in earlier versions. This new implementation uses port 2377 by default.

## Solution

Ensure that incoming port 2377 is available in addition to the existing ports opened for Docker Swarm communication. Your nodes should automatically connect to the controllers after opening the necessary ports, and the required UCP 2.0.0 containers will be deployed by the controllers onto those nodes.
