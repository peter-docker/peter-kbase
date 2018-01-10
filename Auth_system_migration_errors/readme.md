---
type: kbase
version: 1
title: "Auth system migration errors"
summary: "During the upgrade, UCP attempted to migrate team labels and user roles from the old authorization system to the new, collections-based auth system, but something went wrong. To see the full list of errors, run the following command on a UCP manager node: If you'd like to retry the migration, first figure out which node is the Swarm leader:"
dateCreated: "Mon, 24 Jul 2017 21:13:13 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 5
internal: no
author: "joaofnfernandes"
platform:
testedon:
product:
  - EE
tags:
  - upgrading
  - access control
uniqueid: KB000189
---

TESTING TESTING TESTING AND MORE TESTING!!!

## Issue

If UCP is displaying the following warning banner:
```
errors encountered while migrating auth system
```
This means that UCP was upgraded from a version prior to 2.2.0. During the upgrade, UCP attempted to migrate team labels and user roles from the old authorization system to the new, collections-based auth system, but something went wrong.

To see the full list of errors, run the following command on a UCP manager node:

```
$ docker exec -it ucp-kv etcdctl \
    --endpoints https://127.0.0.1:2379 \
    --cert-file /etc/docker/ssl/cert.pem \
    --key-file /etc/docker/ssl/key.pem \
    --ca-file /etc/docker/ssl/ca.pem \
    get /orca/v1/authz_migrated
```

## Resolution

To retry the migration:

1. First figure out which node is the Swarm leader:

    ```
    $ docker node ls
    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
    gpfmosdm99lw9xiz6d4xmsdwo *   node1               Ready               Active              Leader
    gpfmosdm99lw9xiz6d4xmsdwo     node2               Ready               Active
    gpfmosdm99lw9xiz6d4xmsdwo     node3               Ready               Active
    ```

2. Then go to the leader node and run:

    ```
    $ docker exec -it ucp-kv etcdctl \
        --endpoints https://127.0.0.1:2379 \
        --cert-file /etc/docker/ssl/cert.pem \
        --key-file /etc/docker/ssl/key.pem \
        --ca-file /etc/docker/ssl/ca.pem \
        rm /orca/v1/authz_migrated
    ```
3. Then restart the controller on that node:

    ```
    $ docker restart ucp-controller
    ```
