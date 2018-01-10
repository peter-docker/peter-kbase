---
type: kbase
version: 4
title: "How do I configure the KV tuning params on UCP 1.1.2?"
summary: "During the installation process, UCP 1.1.2 has the ability to change two etcd options via the --kv-timeout and --kv-snapshot-count command line arguments. It may also be necessary to restart Docker Engine after doing a join — pay attention to the output of the join command to see whether this will be necessary or not."
dateCreated: "Fri, 29 Jul 2016 19:22:12 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "programmerq"
platform:
  - linux
testedon:
  - ucp-1.1.2
tags:
product:
  - EE
---

For UCP 1.1.2, the default KV parameters are set to `--kv-timeout 1000` (milliseconds) and `--kv-snapshot-count 10000` (simple integer count for the number of etcd changes before doing a snapshot).

Initial testing showed that the performance bottleneck of the UCP system had a definite correlation to these values. The important one in most cases will likely be to increase the snapshot count. Try 20000 or 30000 to start with.

During the installation process, UCP 1.1.2 has the ability to change two etcd options via the `--kv-timeout` and `--kv-snapshot-count` command line arguments. To change these settings on an already existing cluster, following these steps.

> **Warning**: This process is potentially disruptive and should be tried on a non-production instance first. As always, it is good practice to do any potentially disruptive operations during a maintenance window on a production system.

These steps should be done on a UCP 1.1.2 cluster, so upgrade all UCP nodes and controllers to UCP 1.1.2 if you haven't already.

1. First, set the desired values on UCP's backend. SSH in to one of the controllers, and run the following command: 
    
        docker exec -it ucp-kv etcdctl --endpoint https://127.0.0.1:2379 --ca-file /etc/docker/ssl/ca.pem --cert-file /etc/docker/ssl/cert.pem --key-file /etc/docker/ssl/key.pem set /orca/v1/config/kv '{"Timeout":<^>2000<^^>,"SnapshotCount":<^>20000<^^>}'
    
    Adjust the values as you see fit.
2. Next, uninstall one controller by using the `docker/ucp uninstall` command.
3. Copy the `ucp-controller-server-certs` volume contents back to the node.
4. Reinstall the controller by running the `docker/ucp join` command on that node. This newly joined controller will pick up the new settings and use those. Be sure to backup and restore the root ca data as you do your uninstall and join commands. You can check that the reinstall took effect by running the following command: 
    
        $ docker ps --no-trunc | grep ucp.kv | grep 'timeout\|snapshot'
        73e651084617c60451545188233b165e72ef90764d37ce05f286190d25984a35   docker/ucp-etcd:1.1.2         "/bin/etcd --data-dir /data --name orca-kv-104.131.45.27 --listen-peer-urls https://0.0.0.0:12380 --listen-client-urls https://0.0.0.0:2379 --advertise-client-urls https://104.131.45.27:12379 --initial-advertise-peer-urls https://104.131.45.27:12380 --initial-cluster orca-kv-orca-kv-104.131.46.154=https://104.131.46.154:12380,orca-kv-orca-kv-104.236.51.124=https://104.236.51.124:12380,orca-kv-104.131.45.27=https://104.131.45.27:12380 --initial-cluster-state existing --trusted-ca-file /etc/docker/ssl/ca.pem --cert-file /etc/docker/ssl/cert.pem --key-file /etc/docker/ssl/key.pem --client-cert-auth --peer-trusted-ca-file /etc/docker/ssl/ca.pem --peer-cert-file /etc/docker/ssl/cert.pem --peer-key-file /etc/docker/ssl/key.pem --peer-client-cert-auth --heartbeat-interval 200 <^>--election-timeout 2000 --snapshot-count 20000<^^>"
        About a minute ago   Up 3 seconds            2380/tcp, 4001/tcp, 7001/tcp, 0.0.0.0:12380->12380/tcp, 0.0.0.0:12379->2379/tcp   ucp-kv
    
    Here, the `--election-timeout 2000` and `--snapshot-count 20000` arguments are visible.
5. It may also be necessary to restart Docker Engine after doing a join — pay attention to the output of the join command to see whether this will be necessary or not. In most cases, it will be.

6. Lastly, repeat these `ucp uninstall` and `ucp join` steps on each controller.
