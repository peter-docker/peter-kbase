---
type: kbase
version: 3
title: "Unable to create service on an existing network"
summary: "[INTERNAL ONLY] This is meant to be used when the steps provided 'here' do not work. A new service cannot be created on an existing network; however, the same service can not be created on newly created service. The following clean-up steps will clear out all IP address space data and create the network anew -- previously allocated IP addresses no longer in use will not be reallocated when the network is re-created."
dateCreated: "Thu, 07 Sep 2017 22:02:43 GMT"
dateModified: "Mon, 02 Oct 2017 19:40:30 GMT"
dateModified_unix: 1506973230
upvote: 0
internal: yes
author: eiichik
platform:
testedon:
tags:
  - networking
  - INTERNAL
product:
  - EE
---

[INTERNAL ONLY] This is meant to be used when the steps provided "here" do not work.

A new service cannot be created on an existing network; however, the same service can not be created on newly created service. This could be because IP address space on VIP has been exhausted. When this happens, manager nodes should show an error like this.

    could not find an available IP while allocating VIP

## Prerequisites

Before performing these steps, it is recommended to check these first:

* Engine version <pending version check with colleague>

## Steps

The following clean-up steps will clear out all IP address space data and create the network anew -- previously allocated IP addresses no longer in use will not be reallocated when the network is re-created.

1. Stop Docker daemon on all worker nodes.
2. Make sure no worker nodes are running.  
    *** **I****f one or more worker keep running, they can retain the old data and synchronize others prevents the refresh.**
3. Run the following shell commands on all worker nodes: 
    
        rm -rf /var/lib/docker/network/*
        rm -rf /var/lib/docker/swarm/*
4. Restart all worker nodes.
5. Allow a few minutes for cluster to synchronize again.
6. Redeploy the stack.  
    *** **IP address space will be rebuild from scratch when stack is redeployed.**
