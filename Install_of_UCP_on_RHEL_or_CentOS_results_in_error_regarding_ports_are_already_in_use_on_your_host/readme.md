---
type: kbase
version: 5
title: "Install of UCP on RHEL or CentOS results in error regarding ports are already in use on your host"
summary: "Installing UCP on RHEL or CentOS results in the following error: UCP: FATA[0009] The following required ports are already in use on your host - 12385, 443, 2376, 12376, 4789, 12379, 12381, 12380, 12386, 12382, 12383, 12384. First, verify that no other applications are running on any of the required ports. If it is, use the following steps to allow the required ports, restart the firewall, and restart Docker:"
dateCreated: "Thu, 18 May 2017 16:32:06 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
  - linux
testedon:
tags:
  - installing
  - error
  - networking
product:
  - EE
---

## Issue

Installing UCP on RHEL or CentOS results in the following error:

    UCP: FATA[0009] The following required ports are already in use on your host - 12385, 443, 2376, 12376, 4789, 12379, 12381, 12380, 12386, 12382, 12383, 12384.  You may specify an alternative port number to 2376 with the --swarm-port argument.

## Solution

First, verify that no other applications are running on any of the required ports.

Next, check if firewalld is enabled (most likely scenario). If it is, use the following steps to allow the required ports, restart the firewall, and restart Docker:

1. Add ports to open in the firewall: 
    
        for i in 12376 4789 12386 2376 443 12381 12380 12382 12383 12384 12385 12379 ; do
          echo adding $i to the firewall
          firewall-cmd --add-port=$i/tcp --permanent
        done
2. Restart the firewall: 
    
        firewall-cmd –-reload
3. Restart Docker: 
    
        systemctl restart docker
4. Start the UCP installation again.
