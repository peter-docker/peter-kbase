---
type: kbase
version: 10
title: "Why are UCP 2.1.0 nodes continuously reconfiguring?"
summary: "After upgrading to UCP 2.1.0, nodes in the UCP console repeatedly show: On the hosts, the ucp-agent container is continuously restarting. docker ps may take a long time to complete. This issue was tracked by internal engineering issue orca/5917. A fix was released with UCP 2.1.1. For environments running UCP 2.1.0, this behavior can be addressed by issuing the following command from a manager node to update the service definition of the ucp-agent:"
dateCreated: "Fri, 17 Feb 2017 06:51:01 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "trapier"
platform:
testedon:
  - ucp-2.1.0
tags:
product:
  - EE
---

After upgrading to UCP 2.1.0, nodes in the UCP console repeatedly show:

    Node being reconfigured.

## Symptoms

* On the hosts, the `ucp-agent` container is continuously restarting.
* `docker ps` may take a long time to complete.

## Solution

This issue was tracked by internal engineering issue orca/5917. A fix was released with UCP 2.1.1.

For environments running UCP 2.1.0, this behavior can be addressed by issuing the following command from a manager node to update the service definition of the `ucp-agent:`

    docker service update --mount-add type=bind,source=/etc/docker,target=/etc/docker ucp-agent