---
type: kbase
version: 3
title: "Clear auth migration failed banner"
summary:
upvote: 0
internal: no
author: "jacquesg"
platform:
  - windows server
  - linux
  - z systems
testedon:
  - ucp-2.2.4
tags:
  - access control
  - error
  - upgrading
product:
  - EE
---

## Issue

If UCP is displaying the following warning banner:
```
auth migration failed
```
This means that UCP was upgraded from a version prior to 2.2.0. During the upgrade, UCP attempted to migrate team labels and user roles from the old authorization system to the new, collections-based auth system.

If the banner is still persisting after you have verified that the migration was successful, you can use the following procedure to clear it:

1. First figure out which node is the Swarm leader:

    ```
    $ docker node ls
    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
    gpfmosdm99lw9xiz6d4xmsdwo *   node1               Ready               Active              <^>Leader<^^>
    gpfmosdm99lw9xiz6d4xmsdwo     node2               Ready               Active
    gpfmosdm99lw9xiz6d4xmsdwo     node3               Ready               Active
    ```

2. To clear the banner run the following command on the UCP manager leader node:

    ```
    $ docker exec -it ucp-kv etcdctl \
    --endpoints https://127.0.0.1:2379 \
    --cert-file /etc/docker/ssl/cert.pem \
    --key-file /etc/docker/ssl/key.pem \
    --ca-file /etc/docker/ssl/ca.pem \
    set /orca/v1/authz_migrated ‘{“status”:“success”,“version”:“2.2.0 (f1aca4dc1)“,”error”:“”}’ 
    ```

3. Then restart the controller on that node:

    ```
    $ docker restart ucp-controller
    ```

## What's Next

If UCP is displaying the following warning banner:

```
errors encountered while migrating auth system
```

Follow the [Auth system migration errors](https://success.docker.com/KBase/Auth_system_migration_errors) article.
