---
type: kbase
version: 2
title: "How do I add a node back after removing it using the Docker Cloud CLI tool?"
summary: "After removing a node from Docker Cloud using the Docker Cloud CLI tool (docker-cloud node rm <node-UUID>), you might see the following error when trying to add the node back: If this issue is because the configuration file from the previous agent installation is affecting the new installation, assuming that your host is running Ubuntu/Debian, you can run the following commands to fix the issue:"
dateCreated: "Thu, 05 May 2016 07:45:06 GMT"
dateModified: "Tue, 15 Nov 2016 19:12:53 GMT"
dateModified_unix: 1479237173
upvote: 1
internal: no
author: "kenny.lim"
platform:
testedon:
tags:
product:
  - Hub
---

After removing a node from Docker Cloud using the Docker Cloud CLI tool (`docker-cloud node rm <^><node-UUID>`<^^>), you might see the following error when trying to add the node back:

    ERROR: Docker Cloud Agent is already installed in this host
    If the node failed to register properly with Docker Cloud, try to restart the agent by executing:
    service dockercloud-agent restart
    and check the logs at /var/log/dockercloud/agent.log for possible errors. 
    If the problem persists, please contact us at support@docker.com

There error might occur for 2 reasons:

1. Configuration file from the previous agent installation is affecting the new installation.
2. You are trying to add a node, but you have zero node allocations left.

## Resolution

If this issue is because the configuration file from the previous agent installation is affecting the new installation, assuming that your host is running **Ubuntu/Debian**, you can run the following commands to fix the issue:

    sudo apt-get remove dockercloud-agent
    sudo apt-get purge dockercloud-agent

The first command uninstalls the agent, and the second command removes all remaining configuration files from the previous installation.

Once done, try creating the node again by running the command given by Cloud when you click the **Bring your own node** button. The command looks something similar to this:

    curl -Ls https://get.cloud.docker.com/ | sudo -H sh -s 487aa1350fc242eda12ccdcad2818935

If you are attempting to add a node to Docker Cloud without node allocations, you can add nodes by going to your Account Settings: [https://cloud.docker.com/account/#plan](https://cloud.docker.com/account/#plan "https://cloud.docker.com/account/#plan")
