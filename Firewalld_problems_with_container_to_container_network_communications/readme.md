---
type: kbase
version: 3
title: "Firewalld problems with container to container network communications"
summary: "Issue Using ssh to connect from one container to another container on a different node works. However, running ssh from one container to another container on the same node does not work. Solution Firewalld uses the NetworkManager backend. The firewalld commands to resolve this connection issue are similar to the following: firewall-cmd --permanent --zone=trusted --add-interface=docker0 firewall-cmd --permanent --zone=trusted --add-port=4243/tcp"
dateCreated: "Thu, 08 Jun 2017 14:30:45 GMT"
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

## Issue

Using ssh to connect from one container to another container on a different node works. However, running `ssh` from one container to another container on the same node does not work.

## Solution

  
Firewalld uses the NetworkManager backend. The firewalld commands to resolve this connection issue are similar to the following:

    firewall-cmd --permanent --zone=trusted --add-interface=docker0
    firewall-cmd --permanent --zone=trusted --add-port=4243/tcp