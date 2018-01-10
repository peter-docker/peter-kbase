---
title: How do I recover an inquorate DTR <= 2.2.x environment?
internal: yes
type: kbase
author: 
    - "squizzi"
product: EE
testedon: 
    - dtr-2.2.3
    - dtr-2.2.7
    - dtr-2.2.8
tags: 
---
## Issue

The following article will show you how to recover an inquorate DTR RethinkDB when the DTR version is less than 2.3.0.

## Prerequisites

- Verify the underlying RethinkDB cluster is unhealthy and reporting a status of `waiting_on_primary`. You can determine this by hitting the `cluster_status` endpoint, or by using the instructions below:
```
REPLICA_ID=$(docker ps -lf name='^/dtr-rethinkdb-.{12}$' --format {{.Names}} |cut -d- -f3)

docker run -it --rm \
  --net dtr-ol \
  -v dtr-ca-$REPLICA_ID:/ca dockerhubenterprise/rethinkcli:v2.2.0 \
  $REPLICA_ID

> r.db("rethinkdb").table("table_status")
```

You should see similar output in atleast one of the underlying DTR db's.  The `waiting_on_primary` state is the most important here with the fact that only one replica is in a `ready` state.

```
[ { critical: true,
    description: 'Table `dtr2.namespace_team_access` is available for outdated reads, but not up-to-date reads or writes. The following servers are not reachable: dtr_rethinkdb_9d6434996c74',
    id: '47766355-14d5-5ac8-b2ad-75336fb32804',
    info:
     { db: 'dtr2',
       raft_leader: 'dtr_rethinkdb_e9bb3227cb1b',
       shards:
        [ { primary_replicas: [ 'dtr_rethinkdb_22ff36969951' ],
            replicas:
             [ { server: 'dtr_rethinkdb_22ff36969951',
                 state: 'waiting_for_primary' },
               { server: 'dtr_rethinkdb_e9bb3227cb1b', state: 'ready' },
               { server: 'dtr_rethinkdb_9d6434996c74', state: 'disconnected' } ] } ],
```

- Ensure the UCP environment is in a healthy state.
- Use the docker client directly by SSHing to effected nodes, do **NOT** use a UCP bundle.
- Backup the customer's TLS certs and keys.  If a reconfigure is neccessary following recovery those will be overridden.
- Install `jq` or have `jq` available to use on the customer machine.  If `jq` is not in the distribution repository it can be obtained from GitHub with:

```
wget -O jq https://github.com/stedolan/jq/releases/download/jq-1.5/jq-linux64
chmod +x ./jq
cp jq /usr/bin/
```

## Resolution
In the below steps the example replicas:

- Healthy: 2fbc817316e5
- Unhealthy: 1efbf7a2e1e0

will be used, ensure that this replica value is modified to the replica id value's within the environment.

---

To start things off, identify the healthy replica and SSH to that node.  

1. Set up some initial shell variables to make command running easier:

```
export REPLICA_ID=2fbc817316e5
export DTR_VERSION=<DTR_VERSION>
export UCP_PASSWORD=<PASSWORD>
export UCP_URL=<URL>
export DTR_EXTERNAL_URL=<URL>
```

2. Start the RethinkDB CLI: 

```
docker run --rm -it --net dtr-ol -v dtr-ca-$REPLICA_ID:/ca dockerhubenterprise/rethinkcli:v2.2.0 $REPLICA_ID
```

3. Run the following command to see that some tables are currently unhealthy.  This command will iterate through all system tables and pull out the ‘ready_for_writes’ status.  This is simply a command to view the current state of the tables:

```
r.db('rethinkdb').table('table_status').pluck({'status':'ready_for_writes'})
```

4. Emergency repair all of the tables: 

```
r.db('rethinkdb').
table('table_status').
pluck('db', 'name', {'status':'ready_for_writes'}).
forEach(function(table) {
    return r.branch(
        table('status')('ready_for_writes'),
        r.expr({}),
        r.db(table('db')).table(table('name')).reconfigure({'emergencyRepair': 'unsafe_rollback'})
    );
});
```

5. Re-run the above `table_status` command to check the state of each table, `ready_for_writes` should be true for each table:

```
r.db('rethinkdb').table('table_status').pluck({'status':'ready_for_writes'})
```

You can also run the following command to fetch the first row of every table.  This proves that the data is indeed accessible:

```
r.db('rethinkdb').table('table_config').pluck('db','name').map(function(row){ return r.db(row('db')).table(row('name')).nth(0).default({}); })
```

6. Fetch the HA config and copy the fetched config to a file: 

```
r.db('dtr2').table('properties').get('ha.json')('value')
```

The above command will return a large json blob, copy this output.  Exit the RethinkDB CLI with `CTRL+D`, then paste all of the returned text and paste it into a file called `‘ha.json’`.  Remove the trailing single quote and initial single quote in the file to make the json parsable by `jq`. 

7. Using `jq`, remove the unwanted replica from the config and create a new, secondary config:

```
jq 'del(.ReplicaConfig."1")' < ha.json | tr -d '\n' > ha2.json
```

8. Start the RethinkDB CLI again:

```
docker run --rm -it --net dtr-ol -v dtr-ca-$REPLICA_ID:/ca dockerhubenterprise/rethinkcli:v2.2.0 $REPLICA_ID
```

9. **MANUAL STEP**: Copy the contents of `ha2.json` into the following query, replacing `$JSON_HERE`: 

```
r.db('dtr2').table('properties').get('ha.json').update({'value':'$JSON_HERE'})
```

Once ran, exit the RethinkDB CLI with `CTRL+D`.

10. Remove the existing `dtr-rethinkdb` container on the secondary, bad node, currently housing replica `1efbf7a2e1e0`.  SSH to that node and run:

```
docker rm -f dtr-rethinkdb-1efbf7a2e1e0
```

11. SSH back to the now healthy node, housing replica `2fbc817316e5`.  Scale the rethink replicas down using the `rethinkops` command from within the `dtr-rethinkdb` container:

```
docker exec dtr-rethinkdb-$REPLICA_ID rethinkops scale --replicas 1
```

RethinkDB should now be healthy.

### Join Nodes

1. Now that RethinkDB is healthy, DTR bootstrap operations will succeed.  You should now be able to join old nodes back as fresh replicas.  First, we’ll want to ensure none of the old volume data is used in the next set of operations so we’ll clear out all of the running DTR containers and any DTR volumes that might exist.  **Ensure the following commands are ran only on the two other DTR nodes, *NOT* on the node housing replica 2fbc817316e5 as this will result in a complete removal of DTR on those nodes.**

```
docker ps -aq --filter name=dtr | xargs docker rm -f
docker volume ls -q --filter name=dtr | xargs docker volume rm
```

2. Join the remaining nodes one at a time using the `join` command:

```
docker run --rm -it docker/dtr:2.2.3 join --ucp-insecure-tls --ucp-node <NODE_TO_JOIN> --existing-replica-id 2fbc817316e5
```

Recovery should now be complete.

## What's Next

Review the gist written by Viktor Stanchev from the DTR team which this article has been based on: https://gist.github.com/vikstrous/f764c5113834153c8a97f10a667eba82
