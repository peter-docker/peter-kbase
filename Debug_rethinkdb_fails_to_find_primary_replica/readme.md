---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Debug RethinkDB fails to find primary replica in cluster
internal: no             # set to yes to keep it internal-only
comment: "internal comments that will only show up in the GH source file"
type: kbase               # set to customerservice if applicable
author:  adamancini
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.06.2-ee-3
  - ucp-2.2.0
  - dtr-2.3.0
platform:           # Optional. Keep all that apply.
  - linux
  - mac
  - windows
  - windows server
  - z systems
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - daemon
  - error
  - networking
---

## Issue

DTR cluster is unhealthy, and health status logs emit messages like:
```
"2541aa473fde": "Unhealthy replicas: 2541aa473fde; reasons: Rethink replica is currently in state: waiting_for_primary, Rethink replica is currently in state: waiting_for_quorum, Rethink replica is currently in state: waiting_for_primary, Rethink replica is currently in state: waiting_for_primary, Rethink replica is currently in state: waiting_for_primary, Rethink replica is currently in state: waiting_for_primary, Rethink replica is currently in state: waiting_for_primary, Rethink replica is currently in state: waiting_for_primary, Rethink replica is currently in state: waiting_for_primary, Rethink replica is currently in state: waiting_for_primary, Rethink replica is currently in state: waiting_for_primary, Rethink replica is currently in state: waiting_for_quorum, Rethink replica is currently in state: waiting_for_quorum, Rethink replica is currently in state: waiting_for_quorum, Rethink replica is currently in state: waiting_for_quorum, Rethink replica is currently in state: waiting_for_quorum, Rethink replica is currently in state: waiting_for_quorum, Rethink replica is currently in state: waiting_for_quorum, Rethink replica is currently in state: waiting_for_quorum, Rethink replica is currently in state: waiting_for_quorum, Rethink replica is currently in state: waiting_for_quorum",
```

## Prerequisites

Exec into `dtr-rethinkdb-nnnnnnnnnnnn` on all DTR nodes and confirm `config/replica_ids.csv` in the container filesystem contains a comma-separated list of all the DTR replica IDs in the cluster.

Example in a 3-node DTR cluster:

```
$ docker exec dtr-rethinkdb-a2382528a00b cat /config/replica_ids.csv
a2382528a00b,22476e1b69ee,f759e4b0fdaa
```

## Resolution

1. If the `/config/replica_ids.csv` file does NOT contain all of the replica IDs of the cluster members, use a command line editor to make it so:

```
$ docker exec -it dtr-rethinkdb-a2382528a00b vi /config/replica_ids.csv
```

2. Then, `docker stop` all of the rethinkdb containers so they are all stopped at the same time.

```
$ docker stop dtr-rethinkdb-a2382528a00b
$ docker stop dtr-rethinkdb-22476e1b69ee
$ docker stop dtr-rethinkdb-f759e4b0fdaa
```
3. Then, start one rethinkdb container on one DTR node and follow its logs:

```
$ docker start dtr-rethinkdb-a2382528a00b
$ docker logs -f dtr-rethinkdb-a2382528a00b
```

4. Open another terminal, and while watching the logs from the first rethink container, start the remaining rethink containers and check for "Connected to server" messages.

5. If the majority of containers still do not connect to each other, start a test container on the `dtr-ol` network and try to find the IP address of the other replicas from the internal DNS resolver:

```
$ docker run --rm -it --net dtr-ol --entrypoint sh docker/dtr-rethink:$DTR_VERSION
$ getent hosts dtr-rethinkdb-$OTHER_REPLICA_ID
```

6. Repeat this several times, and confirm that the reply is consistently `NXDOMAIN` if that `dtr-rethinkdb` container is not currently running or a single IP address if that replica id is currently running.

If there is a DNS entry for a `dtr-rethinkdb` container that is not currently running or there are multiple DNS entries for the same dtr-rethinkdb container name, then there is an issue with overlay network service discovery which can only be cleared by stopping the Docker daemon on all DTR nodes such that the daemons are stopped at the same time, then starting the Docker daemon on all nodes.
