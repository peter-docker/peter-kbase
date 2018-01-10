---
type: kbase
version: 7
title: "How can I remove a failed controller node from a UCP cluster?"
summary: "To remove a failed controller from a UCP cluster, use the following steps. If the failed node's Docker daemon is still running and reachable on the same network as the UCP cluster, Docker Swarm might sometimes reconnect to the same network again, and the node might be relisted with the following command: Alternatively, you can terminate the Docker daemon on that failed node, and then remove it again with the following command:"
dateCreated: "Thu, 14 Jul 2016 18:24:07 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 2
internal: no
author: "sabin.basyal"
platform:
testedon:
  - ucp-1.1.1
  - ucp-1.1.2
tags:
product:
  - EE
---

To remove a failed controller from a UCP cluster, use the following steps.

First, go to one of your controller nodes and check if the failed controller node still shows up:

    sudo docker exec -it ucp-kv etcdctl --endpoint [https://127.0.0.1:2379](https://127.0.0.1:2379 "https://127.0.0.1:2379") --ca-file /etc/docker/ssl/ca.pem --cert-file /etc/docker/ssl/cert.pem --key-file /etc/docker/ssl/key.pem ls /orca/v1/controllers

You should find the failed node. Then run this command to actually remove it from the UI:

    sudo docker exec -it ucp-kv etcdctl --endpoint [https://127.0.0.1:2379](https://127.0.0.1:2379 "https://127.0.0.1:2379") --ca-file /etc/docker/ssl/ca.pem --cert-file /etc/docker/ssl/cert.pem --key-file /etc/docker/ssl/key.pem rm /orca/v1/controllers/<node-ip>

This should get rid of the controller node in the UI. However, you might still see references to the failed node when the ucp-controller container tries to create a PKI. In that case, remove the failed node permanently using these two commands:

    sudo docker exec -it ucp-kv etcdctl --endpoint [https://127.0.0.1:2379](https://127.0.0.1:2379 "https://127.0.0.1:2379") --ca-file /etc/docker/ssl/ca.pem --cert-file /etc/docker/ssl/cert.pem --key-file /etc/docker/ssl/key.pem rm /orca/v1/config/clusterca_<node-ip>:12381

    sudo docker exec -it ucp-kv etcdctl --endpoint [https://127.0.0.1:2379](https://127.0.0.1:2379 "https://127.0.0.1:2379") --ca-file /etc/docker/ssl/ca.pem --cert-file /etc/docker/ssl/cert.pem --key-file /etc/docker/ssl/key.pem rm /orca/v1/config/clientca_<node-ip>:12382

If the failed node's Docker daemon is still running and reachable on the same network as the UCP cluster, Docker Swarm might sometimes reconnect to the same network again, and the node might be relisted with the following command:

    sudo docker exec -it ucp-kv etcdctl --endpoint [https://127.0.0.1:2379](https://127.0.0.1:2379 "https://127.0.0.1:2379") --ca-file /etc/docker/ssl/ca.pem --cert-file /etc/docker/ssl/cert.pem --key-file /etc/docker/ssl/key.pem ls /docker/nodes

If that happens, you can rejoin the node again, and then remove it once more. Alternatively, you can terminate the Docker daemon on that failed node, and then remove it again with the following command:

    sudo docker exec -it ucp-kv etcdctl --endpoint [https://127.0.0.1:2379](https://127.0.0.1:2379 "https://127.0.0.1:2379") --ca-file /etc/docker/ssl/ca.pem --cert-file /etc/docker/ssl/cert.pem --key-file /etc/docker/ssl/key.pem rm /docker/nodes/<node-ip>

**NOTE** This procedure has been tested to work with UCP `1.1.2`. Newer versions may not work exactly like this.