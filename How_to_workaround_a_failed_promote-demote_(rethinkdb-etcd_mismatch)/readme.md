---
type: kbase
version: 4
title: "How to workaround a failed promote/demote (rethinkdb/etcd mismatch)"
summary: "Workaround failed promote/demote (rethinkdb/etcd mismatch) within UCP [ERROR] Unable to promote node to controller: unsuccessful node promote request: node promotion failed: etcd and rethinkdb cluster healthcheck failed: mismatch between etcd (2) and rethinkdb (1) replica count Determining the actual number of expected controllers in the system, then run the following command (replacing '2' with the actual number for your environment)"
dateCreated: "Fri, 11 Nov 2016 18:31:41 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 2
internal: no
author: "justinnevill"
platform:
testedon:
tags:
product:
  - EE
---

# Workaround failed promote/demote (rethinkdb/etcd mismatch) within UCP

Under certain circumstances promotion or demotion may get stuck, resulting in a manager node reporting `[Pending] Node is being reconfigured (11%)`

![Details](./images/promote-1.png)

If you view the `Agent Log` the following failure information will be reported

    [ERROR] Unable to promote node to controller: unsuccessful node promote request: node promotion failed: etcd and rethinkdb cluster healthcheck failed: mismatch between etcd (2) and rethinkdb (1) replica count

![Log](./images/promote-2.png)

## Recovery Procedure

Determining the **actual** number of expected controllers in the system, then run the following command (replacing "2" with the actual number for your environment)

    docker ps --filter name=ucp-kv -q | wc -l

    docker exec ucp-auth-api enzi $(docker inspect --format '{{ index .Args 0 }}' ucp-auth-api) --debug reconfigure-db --num-replicas 2
