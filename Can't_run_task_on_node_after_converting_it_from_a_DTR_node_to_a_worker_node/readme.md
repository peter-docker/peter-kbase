---
type: kbase
version: 2
title: "Can't run task on node after converting it from a DTR node to a worker node"
summary: "If you uninstalled DTR from a node and are unable to run tasks on it as a worker node, there might still be cluster configuration data somewhere that still thinks the former DTR node is still a manager node. To convert the former DTR node to a worker node, demote the node, remove it from the cluster, and then re-add it using the following steps: Then add the node to this swarm as a worker, using the token from the previous command and the correct IP of the swarm manager:"
dateCreated: "Wed, 10 May 2017 00:27:45 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 0
internal: no
author: "bryceryan-docker"
platform:
testedon:
tags:
product:
  - EE
uniqueid: KB000194
---

TESTING TESTING TESTING AND MORE TESTING!!!

If you uninstalled DTR from a node and are unable to run tasks on it as a worker node, there might still be cluster configuration data somewhere that still thinks the former DTR node is still a manager node. This can happen if DTR was uninstalled incorrectly or some configuration was not done correctly.  
  
To convert the former DTR node to a worker node, demote the node, remove it from the cluster, and then re-add it using the following steps:

1. Drain the node using a command similar to the following: 
    
        docker node update --availability drain <^><docker_node><^^>
        
2. Demote the node from manager status to worker status using a command similar to the following: 
    
        docker node demote <^><docker_node><^^>
3. On the command line of the node itself, issue the following command to have it leave the swarm: 
    
        docker swarm leave
4. Now rejoin the node to the swarm using the join token.  
    First, get the join token by running the following from a command line on a UCP node: 
    
        docker swarm join-token worker
    Then add the node to this swarm as a worker, using the token from the previous command and the correct IP of the swarm manager: 
    
        docker swarm join --token <^><your_join_token><^^> <^><swarm_manager_ip><^^>:2377
    Run the `join` command that was given to you on the node.
5. You should now be able to run workloads on that node.
