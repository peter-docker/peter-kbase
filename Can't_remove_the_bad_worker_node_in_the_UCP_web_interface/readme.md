---
title: "Can't remove the bad worker node in the UCP web interface"
internal: no
comment: internal comments that will only show up in the GH source file
type: kbase
author: "fotios.tsiadimos"
product:
  - ee
testedon:
  - ee-17.06.2-ee-5
  - ucp-2.2.3
platform:
  - linux
tags:
  - error
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
uniqueid: KB000193
---

TESTING TESTING TESTING AND MORE TESTING!!!

## Issue

Can't remove the bad worker node in the UCP web interface:

```
Error response from daemon: rpc error: code = 9 desc = node 52efya1qv8phpe25v6vhdaaso
```

## Resolution

Use the following steps to remove the bad worker node via the command line:

1. From the client bundle remove the worker node:

   ```
   $ docker node rm --force <^><worker node><^^>
   ```
2. From the worker node leave swarm:

    ```
    $ docker swarm leave
    ```
3. Then join back the worker back to swarm:

    ```
    $  docker swarm join --token <^><token id><^^>
    ```

## What's Next

Please review the [Docker docs about using `docker node rm`](https://docs.docker.com/engine/reference/commandline/node_rm/#remove-a-stopped-node-from-the-swarm ).


