---
type: kbase
version: 9
title: "How can I assign a service to Windows nodes only?"
summary: "To run a service only on a specific subsets of nodes, use labels and the constraint parameter. First, all Windows nodes must be labeled using these steps for each Windows node: By adding constraints, you can selectively assign a service to specific nodes using the following steps: Make sure only nodes with the os==windows label were assigned: List of all supported options for Windows can be found in the Docker docs."
dateCreated: "Tue, 28 Mar 2017 18:51:41 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "eiichik"
platform:
  - windows server
testedon:
  - cse-1.13.1-cs1
tags:
product:
  - EE
---

To run a service only on a specific subsets of nodes, use labels and the constraint parameter.

## Prerequisites

Before performing these steps, you must meet the following requirements:

* Nodes are running on version 1.13 or later

## Step 1: Label the Windows Nodes

First, all Windows nodes must be labeled using these steps for each Windows node:

1. Open the following configuration file for the Docker daemon. If it doesn't exist, create the file. 
    
        C:\ProgramData\docker\config\daemon.json
2. Add labels to mark it as Windows node: 
    
        {
            "labels": ["os=windows"]
        }
3. Restart Docker Engine: 
    
        Restart-Service docker
4. Verify that the label is in place: 
    
        > Docker info    
        ...
        Labels:
        <^>os=windows<^^>
        ...

## Step 2: Start the Service with Contraints

By adding constraints, you can selectively assign a service to specific nodes using the following steps:

1. Create a service with constraint: 
    
        > docker service create <^>--constraint engine.labels.os==windows<^^> --name winonly microsoft/nanoserver cmd /c “ping docker.com” -n 30
2. Make sure only nodes with the <^>os==windowslabel<^^> were assigned: 
    
        > docker service ps winonly 
         ID            NAME            IMAGE                        NODE   DESIRED STATE  CURRENT...
         itm33*******  winonly.1       microsoft/nanoserver:latest  win00  Running        Running...

## What's Next

There are many more options that you can set for Docker daemon. List of all supported options for Windows can be found [in the Docker docs](https://docs.docker.com/engine/reference/commandline/dockerd/#windows-configuration-file "https://docs.docker.com/engine/reference/commandline/dockerd/#windows-configuration-file").
