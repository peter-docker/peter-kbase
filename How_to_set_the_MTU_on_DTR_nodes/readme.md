---
type: kbase
version: 3
title: "How to set the MTU on DTR nodes"
summary: "The way to configure the default bridge network differs from the way to configure the individual custom networks. docker network rm dtr-br # on every machine, through the UCP client bundle by prefixing the network name with the name of the machine or through UCP's UI docker network rm dtr-ol # once for the whole cluster After stopping all containers and deleting both networks, re-create the dtr-br network on all machines with the following command:"
dateCreated: "Wed, 29 Jun 2016 21:17:11 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "sabin.basyal"
platform:
testedon:
tags:
  - networking
product:
  - EE
---

Docker allows you to set MTUs on all networks it creates. The way to configure the default bridge network differs from the way to configure the individual custom networks.

You can configure the bridge network using the `--mtu` flag in the daemon's start up arguments. Restarting the daemon is enough to make these changes take effect. This will work for all UCP containers.

To configure custom networks you must delete them and create them again. If you've already installed DTR, you have two options:

* Either uninstall DTR, re-create the networks, then install it again or
* Stop all DTR containers, re-create the networks, then start DTR containers again.

The second case is described here.

Run the following for each replica to stop all DTR containers. You can do this with the UCP client bundle, UCP's UI, or on each individual machine:

    docker kill dtr-etcd-<^><replica_id><^^> dtr-rethinkdb-<^><replica_id><^^> dtr-registry-<^><replica_id><^^> dtr-api-<^><replica_id><^^> dtr-nginx-<^><replica_id><^^>

There are two networks to re-create. One is the overlay network that spans the whole cluster. The other is the per-node `dtr-br` local bridge network.

Delete the two networks:

    docker network rm dtr-br # on every machine, through the UCP client bundle by prefixing the network name with the name of the machine or through UCP's UI
    docker network rm dtr-ol # once for the whole cluster

After stopping all containers and deleting both networks, re-create the `dtr-br` network on all machines with the following command:

    docker network create --opt=com.docker.network.driver.mtu=<^><your_mtu_value><^^> dtr-br

Also create the overlay network (only once on any node) with the following command:

    docker network create --driver overlay --opt=com.docker.network.driver.mtu=<^><your_mtu_value><^^> dtr-ol

Finally, start all DTR containers again, and the MTUs should be set on all networks:

    docker start dtr-etcd-<^><replica_id><^^> dtr-rethinkdb-<^><replica_id><^^> dtr-registry-<^><replica_id><^^> dtr-api-<^><replica_id><^^> dtr-nginx-<^><replica_id><^^>
