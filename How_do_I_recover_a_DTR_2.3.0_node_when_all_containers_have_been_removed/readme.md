---
title: How do I recover a DTR 2.3.0+ node when all containers have been removed?
internal: no
type: kbase
dateModified: "Wed, 11 Oct 2017 14:52:30 GMT"
dateModified_unix: 1507751550
author:
    - "squizzi"
product: ee
platform:
    - linux
    - windows
testedon: 
    - ee-17.06.2-ee-2
    - dtr-2.3.0
tags:
---
## Issue

If an issue arises where the underlying DTR containers providing service are removed via a `docker rm`, you may need to restore the node to get the functionality back.  If you run a `docker ps -a` and see no `dtr-` containers present, use the instructions below to recover the problematic DTR node.

## Prerequisites

To perform the following you must: 

* Be running Docker DTR 2.3.0 or higher
* Have **all** of the DTR volumes intact, visible by running a `docker volume ls`
    * `dtr-ca-<replica_id>`
    * `dtr-etcd-<replica_id>`
    * `dtr-notary-<replica_id>`
    * `dtr-registry-<replica_id>`
    * `dtr-rethink-<replica_id>`
    * `dtr-nfs-registry-<replica-id>`

## Resolution

If any of the underlying DTR containers still exist when running a `docker ps -a` jump to step 6 of this solution.

1. Grab the underlying rethink replica id from the existing DTR volumes. **If the volumes do not exist, recovery cannot be completed.**
    ```
    REPLICA_ID=$(docker volume ls --filter name=dtr | awk '{print $NF}' | cut -d- -f3 | grep -vi name | head -1)
    ```
2. Specify the DTR version tag you are recovering against, for example, 2.3.0 and run a fake `dtr-nginx` container with `alpine`:
    ```
    docker run -it -e "DTR_VERSION"="2.3.0" --name dtr-nginx-$REPLICA_ID alpine
    ```
3. Wait 15 seconds:
    ```
    sleep 15
    ```
4. Run a reconfigure on the node, once again specifying your DTR version, in the example below 2.3.0 is used:
    ```
    docker run --rm -it docker/dtr:2.3.0 reconfigure --ucp-insecure-tls
    ```
    The reconfigure will resolve all of the underlying DTR containers **except** for `dtr-nginx` as it believes it already exists.  
5. To fix the issue with `dtr-nginx` remove the container:
    ```
    docker rm dtr-nginx-$REPLICA_ID
    ```
6. Then run the reconfigure once more:
    ```
    docker run --rm -it docker/dtr:2.3.0 reconfigure --ucp-insecure-tls 
    ```

The node should now be recovered.
