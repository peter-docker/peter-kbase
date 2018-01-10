---
type: kbase
version: 3
title: "RethinkDB cluster warnings"
summary: "User and organization data for Docker EE is stored in a RethinkDB database which is replicated across all manager nodes in the UCP cluster. UCP constantly monitors the health of the replicas and alerts when one or more of the replicas are unhealthy. There are currently m RethinkDB replicas in the system, but only n of them are healthy. Since this problem is in the distributed database of UCP, you should not add or remove nodes from your cluster until the problem is solved."
dateCreated: "Mon, 24 Jul 2017 20:53:54 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "joaofnfernandes"
platform:
testedon:
tags:
product:
  - EE
---

User and organization data for Docker EE is stored in a [RethinkDB](https://www.rethinkdb.com/) database which is replicated across all manager nodes in the UCP cluster.

UCP constantly monitors the health of the replicas and alerts when one or more of the replicas are unhealthy.

### Rethinkdb cluster unhealthy: `n` of `m` replicas are healthy.

There are currently `m` RethinkDB replicas in the system, but only `n` of them are healthy.

1. You can look at the logs by running
    ```
    docker logs ucp-controller 2>&1 | grep "rethinkDB table status"
    ```
1. You can also try to troubleshoot by using the instructions posted [here](https://docs.docker.com/datacenter/ucp/2.1/guides/admin/monitor-and-troubleshoot/troubleshoot-configurations/#rethinkdb-database).
`. If you cannot find what is wrong, [collect the support dump and file a support ticket](https://docs.docker.com/datacenter/ucp/2.1/guides/get-support).

Since this problem is in the distributed database of UCP, you should not add or remove nodes from your cluster until the problem is solved. Otherwise, it can lead to data loss.
