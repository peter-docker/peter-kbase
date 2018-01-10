---
type: kbase
version: 5
title: "UCP is unable to install because of a port conflict"
summary: "If the UCP installation fails to complete with a port-check error, please make sure these ports are available and that the host's --advertise-addr, if provided during the join operation, is routable from the host itself. Finally, if the node has changed IP addresses since the docker swarm join operation was performed and you are observing port check failures during promotion to manager, the node will need to be rejoined to the cluster."
dateCreated: "Sat, 22 Oct 2016 00:06:15 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 2
internal: no
author: "justinnevill"
platform:
testedon:
  - ucp-2.0.0
tags:
  - installing
  - error
product:
  - EE
---

> This only affects UCP 2.0.0 or later versions.

UCP requires a specific range of ports to install, which are described in detail at the [Systems Requirements](https://docs.docker.com/ucp/installation/system-requirements) page. If the UCP installation fails to complete with a port-check error, please make sure these ports are available and that the host's `--advertise-addr`, if provided during the join operation, is routable from the host itself.

Finally, if the node has changed IP addresses since the `docker swarm join` operation was performed and you are observing port check failures during promotion to manager, the node will need to be rejoined to the cluster. This can be accomplished by first removing the node with the following sequence of operations, performed from another manager node:

    docker node demote
    docker node rm --force

Then we can safely rejoin the new node by running the following operations:

    docker swarm leave
    docker swarm join ...
