---
type: kbase
version: 11
title: "How do I change the UCP 2.0 Controller Port?"
summary: 
dateCreated: "Mon, 05 Jun 2017 14:31:21 GMT"
dateModified: "Thu, 19 Oct 2017 15:43:38 GMT"
dateModified_unix: 1508427818
upvote: 0
internal: no
author: "trapier"
platform:
testedon:
  - ucp-2.0.0
tags:
product:
  - EE
---
## Issue
By default, UCP listens on port `443`. This article explains how you can change the default UCP 2.0 controller port to your desired value through either the UCP Web UI or the command line.

## Prerequisites

Before performing these steps, you must meet the following requirements:

- Universal Control Plane 2.0.x

## Resolution
### Via the UCP Web UI
To change the port UCP is listening on, navigate in the Web UI to **Admin Settings** > **Cluster Configuration** > **Controller Port**. This setting was introduced in UCP 2.0.0. Applying a change to this setting will result in UCP controllers restarting and may briefly interrupt UCP availability. If using a [load balancer for UCP](https://docs.docker.com/datacenter/ucp/2.1/guides/admin/configure/use-a-load-balancer/ "https://docs.docker.com/datacenter/ucp/2.1/guides/admin/configure/use-a-load-balancer/"), the load balancer will also need to be reconfigured to accommodate this change.

![](./images/2017-06-15-151615_2186x973_scrot.png)

### Via the Command Line (CLI)
Alternatively, to change the configuration via the command line, issue the following command from one of the manager nodes:

    docker service update --env-add CONTROLLER_PORT=<^>port_num<^^> ucp-agent
    
## Related Article

If you are on UCP 2.2, you may refer to the article [How do I change the UCP 2.2 controller port?](https://success.docker.com/article/How_do_I_change_the_UCP_2.2_controller_port).
