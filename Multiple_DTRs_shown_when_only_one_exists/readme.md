---
type: kbase
version: 3
title: "Multiple DTRs shown when only one exists"
summary: "When changing the name of DTR through the reconfigure command, a stale DTR entry may be found in the UCP UI under Admin Settings > DTR. The stale entry in the following image has been highlighted. Perform the following steps using etcdctl to remove the stale entry. Define a temporary alias that can be used to run etcdctl for convenience:"
dateCreated: "Wed, 28 Jun 2017 16:39:54 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "trapier"
platform:
testedon:
  - ucp-2.1.5
tags:
product:
  - EE
---

When changing the name of DTR through the `reconfigure` command, a stale DTR entry may be found in the UCP UI under **Admin Settings** > **DTR**. The stale entry in the following image has been highlighted.
![d5358184-0e52-11e7-83bb-ea114c8e19fef.png](./images/d5358184-0e52-11e7-83bb-ea114c8e19fef.png)
This behavior has been reported to Docker Engineering under issue id `dhe-deploy/4918`.

Perform the following steps using `etcdctl` to remove the stale entry.

#### On one manager node:

1. Define a temporary alias that can be used to run `etcdctl` for convenience:
    
        alias etcdctl="docker exec -it ucp-kv /bin/etcdctl --ca-file /etc/docker/ssl/ca.pem --cert-file /etc/docker/ssl/cert.pem --key-file /etc/docker/ssl/key.pem --endpoints https://localhost:2379"

2. List all known DTR entries in `etcd`:
    
        etcdctl ls /orca/v1/config/ |grep trustedregistry

3. Remove the defunct entry:
    
        etcdctl rm /orca/v1/config/trustedregistry_name.of.dtr.from.step.2

4. Confirm the entry has been removed:
    
        etcdctl ls /orca/v1/config/ |grep trustedregistry

#### On each manager, one at a time:

1. Restart the `ucp-controller` container:
    
        docker restart ucp-controller