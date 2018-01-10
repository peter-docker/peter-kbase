---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: "When adding a new replica with the 'dtr join' command, the following error occurs: \"It's impossible to have more replicas of the data than there are servers.\""
internal: no             # set to yes to keep it internal-only
comment: "internal comments that will only show up in the GH source file"
type: kbase               # set to customerservice if applicable
author:  "squizzi"
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
platform:           # Optional. Keep all that apply.
  - linux
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - error
  - installing
  - upgrading
---
## Issue

When adding a new DTR replica using the `dtr join` command, the following error occurs:

```
Error changing rethink replication factor: unable to configure table "layers": unable to reconfigure table replication: gorethink: Can't put 5 replicas on servers with the tag `default` because there are only 4 servers with the tag `default`. It's impossible to have more replicas of the data than there are servers. in: r.DB("jobrunner").Table("jobs").Reconfigure(shards=1, replicas=5)
```

## Prerequisites

This issue occurs on the following products: 

- Docker Trusted Registry

## Root Cause

This issue occurs when network flapping issues are occuring which results in the newly added replica disconnecting periodically during the table reconfigure stage of the replica join.  Check the logs of existing `dtr-rethink` containers for evidence of this. For example:

```
Disconnected from server "dtr_rethinkdb_021be00156b4" f262d163-0cbd-443e-89ad-3b128e3c5ac9
Connected to server "dtr_rethinkdb_021be00156b4" f262d163-0cbd-443e-89ad-3b128e3c5ac9
...
Disconnected from server "dtr_rethinkdb_021be00156b4" f262d163-0cbd-443e-89ad-3b128e3c5ac9
Connected to server "dtr_rethinkdb_021be00156b4" f262d163-0cbd-443e-89ad-3b128e3c5ac9
~~~

When this happens, the number of replicas across all of the tables in the DTR rethink cluster can become inconsistent resulting in situations where some tables contain all of the replicas including the newly joined one, while others will lack the newly joined replica. This can be verified using the `cluster_status` endpoint in DTR by navigating to `https://<dtr-external-url>/api/v0/meta/cluster_status`.

In the following example 5 replicas are expected, but a subset of the tables in the `dtr2` db have 4 replicas:

~~~
"db": "dtr2",
     "id": "4fa2a8ad-aa20-4881-88ad-d95e022692f2",
     "name": "repositories",
     "raft_leader": "dtr_rethinkdb_f8d38c76b4c2",
     "shards": [
      {
       "primary_replicas": [
        "dtr_rethinkdb_f8d38c76b4c2"
       ],
       "replicas": [
        {
         "server": "dtr_rethinkdb_f8d38c76b4c2",
         "state": "ready"
        },
        {
         "server": "dtr_rethinkdb_000000055603",
         "state": "ready"
        },
        {
         "server": "dtr_rethinkdb_3e53b593c962",
         "state": "ready"
        },
        {
         "server": "dtr_rethinkdb_000000055605",
         "state": "ready"
        },
        {
         "server": "dtr_rethinkdb_000000055604",
         "state": "ready"
        }

"db": "jobrunner",
     "id": "644c352a-bf9e-4818-89a9-54a42f10138d",
     "name": "jobs",
     "raft_leader": "dtr_rethinkdb_000000055604",
     "shards": [
      {
       "primary_replicas": [
        "dtr_rethinkdb_f8d38c76b4c2"
       ],
       "replicas": [
        {
         "server": "dtr_rethinkdb_f8d38c76b4c2",
         "state": "ready"
        },
        {
         "server": "dtr_rethinkdb_000000055603",
         "state": "ready"
        },
        {
         "server": "dtr_rethinkdb_000000055605",
         "state": "ready"
        },
        {
         "server": "dtr_rethinkdb_000000055604",
         "state": "ready"
        }
       ]
      }
     ],
     "status": {
      "all_replicas_ready": true,
      "ready_for_outdated_reads": true,
      "ready_for_reads": true,
      "ready_for_writes": true
     }
```

Work is being done to make the `join` command more reliable for future versions of DTR. 

## Resolution

If you encounter the above error during a `dtr join`, immediately perform a remove of the newly added replica then re-issue the `join` command when the network is no longer exhibiting issues: 

```
$ docker run --rm -it docker/dtr:<^><dtr_version><^^> remove --replica-id <^><newly_added_replica_id><^^> --existing-replica-id <known_healthy_replica_id>
```
