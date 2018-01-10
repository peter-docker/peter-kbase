---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Unhealthy DTR replica
internal: no             
comment: ""
type: kbase               
author:  hannahagee
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
  - ee-17.03.2-ee-5
  - ucp-2.1.5
  - dtr-2.3.4
platform:           
  - linux
tags:               
  - error
---
## Issue

Removed a DTR replica instance by running a `destroy` instead of a `remove`: 
```
$ docker run -it --rm \ 
docker/dtr:2.3.4 destroy \ 
--ucp-insecure-tls 
```

When trying to create a DTR replica it fails: 
```
$ docker run -it --rm docker/dtr:2.3.4 join --ucp-node cdlcldpoc0010 --ucp-insecure-tls 
```

```	
FATA[0025] This DTR has replicas that are not working correctly. Make sure existing DTR replicas are healthy before joining new ones. To ignore this warning, use --unsafe-join. Error: Unhealthy replicas: b9954c115ff1; reasons: Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected, Rethink replica is currently in state: disconnected 
FATA[0047] Failed to execute phase2: Problem running container 'dtr-phase2' from image 'docker/dtr:2.3.4': non-zero return code returned from docker/dtr:2.3.4 : 1
```

It looks like the DTR command is trying to connect to a DTR instance that was removed: 

`b9954c115ff1`

## Resolution

1. Attempt to remove the unhealthy replica: 

    ```
    $ docker run -it --rm docker/dtr:2.3.4 remove --replica-ids b9954c115ff1 --debug --ucp-insecure-tls 
    ```
2. Check for any issues: 

    ```
    $ docker run -it --rm --net dtr-ol -v dtr-ca-$REPLICA_ID:/ca dockerhubenterprise/rethinkcli:v2.2.0 $REPLICA_ID 
    > r.db("rethinkdb").table("current_issues") 
    [] 
    ```
3. Attempt to re-join:

    ```
    $ docker run -it --rm docker/dtr:2.3.4 join --ucp-node NODEID --ucp-insecure-tls --debug 
    ```
