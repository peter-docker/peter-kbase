---
type: kbase
version: 2
title: "Trouble stopping Docker Cloud from spawning nodes on AWS after adding/creating a swarm"
summary: "Trouble stopping Docker Cloud from spawning nodes on AWS after adding/creating a swarm. Unregistered swarm from Docker Cloud and removed the access permissions in the settings. Unregistering a swarm from Docker Cloud will not terminate the swarm. Docker for AWS creates auto scaling groups, so you will need to destroy the stack created in the CloudFormation section. Terminating a swarm from Docker Cloud will terminate the swarm on the corresponding provider."
dateCreated: "Wed, 07 Jun 2017 15:09:39 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "hannahagee"
platform:
testedon:
tags:
product:
  - Hub
---

**Issue**

Trouble stopping Docker Cloud from spawning nodes on AWS after adding/creating a swarm.

**Diagnostics**

Unregistered swarm from Docker Cloud and removed the access permissions in the settings. They simply get re-created when removed in AWS.

**Resolution**

Unregistering a swarm from Docker Cloud will not terminate the swarm. You must directly access the swarm to terminate it.

> Docker for AWS creates auto scaling groups, so you will need to destroy the stack created in the CloudFormation section. As of now, from Docker Cloud we do not remove swarms in the provider.

Terminating a swarm from Docker Cloud will terminate the swarm on the corresponding provider.