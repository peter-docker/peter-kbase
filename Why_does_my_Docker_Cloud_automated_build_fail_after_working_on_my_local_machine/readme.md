---
type: kbase
version: 2
title: "Why does my Docker Cloud automated build fail after working on my local machine?"
summary: "If your build keeps failing on Docker Cloud Automated Build but it builds successfully on your local machine, you most like are using a node with insufficient memory to build your image. On Cloud, builds are performed on one of your nodes on Docker Cloud, whereas builds triggered on Docker Hub are built on dedicated build clusters for Docker Hub."
dateCreated: "Mon, 28 Mar 2016 13:52:39 GMT"
dateModified: "Mon, 14 Nov 2016 22:25:28 GMT"
dateModified_unix: 1479162328
upvote: 1
internal: no
author: "fauzan.ariffin"
platform:
testedon:
tags:
  - Docker Cloud
product:
  - Hub
---

## Issue

If your build keeps failing on Docker Cloud Automated Build but it builds successfully on your local machine, you most like are using a node with insufficient memory to build your image.

The automated build feature on Cloud behaves differently from Docker Hub. On Cloud, builds are performed on one of your nodes on Docker Cloud, whereas builds triggered on Docker Hub are built on dedicated build clusters for Docker Hub.

This is an example error message during the building process:

    g++: internal compiler error: Killed (program cc1plus)

## Resolution

To solve this issue, add a new node with larger specs (at least 4GB of memory) and tag it with the 
builderdeploy tag, so that all automated builds trigger on Cloud are built using that node:

1. Launch a new node cluster
2. Add the tag 
    builderunder **Deploy tags**
3. Make sure to select a node type with at least 4GB of RAM
4. Launch node cluster
5. Rebuild your image

To read more about Automated Builds on Cloud: <https://docs.docker.com/docker-cloud/feature-reference/automated-build/>.
