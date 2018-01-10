---
type: kbase
version: 6
title: "How do I change the docker gwbridge address?"
summary: "If the IP address of the interface docker_gwbridge conflicts with an address on your network, it can be changed on a host-by-host basis. To alter the subnet of this interface, create the subnet, stop the containers attached to the docker_gwbridge network, remove the network, re-add it with the desired address, and restart the containers attached to it as follows (must be done on a host-by-host basis):"
dateCreated: "Wed, 21 Dec 2016 17:07:39 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 8
internal: no
author: "trapier"
platform:
  - linux
testedon:
tags:
  - networking
product:
  - EE
---

The `docker_gwbridge` interface provides default gateway functionality for all containers attached to the `docker_gwbridge` network. It is created on each Docker host when they are joined to a swarm cluster.

If the IP address of the interface `docker_gwbridge` conflicts with an address on your network, it can be changed on a host-by-host basis.

> **Warning:** Modifying an existing member of a cluster involves stopping the containers attached to the `docker_gwbridge` network.

To alter the subnet of this interface, create the subnet, stop the containers attached to the `docker_gwbridge` network, remove the network, re-add it with the desired address, and restart the containers attached to it as follows (must be done on a host-by-host basis):

1. List all containers on the `docker_gwbridge` network attached to this host:
    
        docker network inspect --format '{{range $key, $val := .Containers}} {{$key}}{{end}}' docker_gwbridge | \
        xargs -d' ' -I {} -n1 docker ps --format {{.Names}} -f id={}
    
    If it is operationally acceptable to stop these containers, save their names to a convenience variable called so they can be restarted later. If it is not acceptable to stop these containers, do not proceed with the procedure.
    
        gwbridge_users=$(docker network inspect --format '{{range $key, $val := .Containers}} {{$key}}{{end}}' docker_gwbridge | \
        xargs -d' ' -I {} -n1 docker ps --format {{.Names}} -f id={})

2. Stop all containers using the `docker_gwbridge` network using the convenience variable defined in step 1.
    
        echo "$gwbridge_users" | xargs docker stop

3. Remove the `docker_gwbridge` network:
    
        docker network rm docker_gwbridge

4. Define two shell variables, `SUBNET` and `GATEWAY`, with the desired network and gateway address:
    
        SUBNET=<^>172.20.0.0/20<^^>
        GATEWAY=<^>172.20.0.1<^^>

5. Recreate the `docker_gwbridge` network using the desired prefix, setting to the desired values:
    
        docker network create  \
        --subnet=${SUBNET}     \
        --gateway ${GATEWAY}   \
        -o com.docker.network.bridge.enable_icc=false \
        -o com.docker.network.bridge.name=docker_gwbridge \
        -o com.docker.network.bridge.enable_ip_masquerade=true \
        docker_gwbridge

6. (Optional) Confirm the settings on `docker_gwbridge`:
    
        docker network inspect docker_gwbridge --format '{{range $k, $v := index .IPAM.Config 0}}{{.| printf "%s: %s " $k}}{{end}}'

7. Start containers that were stopped in step 1:
    
        echo "$gwbridge_users" | xargs docker start
