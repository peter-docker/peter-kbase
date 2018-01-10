---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Docker hangs when attempting to bind a large number of ports
type: kbase
author:  adamancini
product: 
  - EE
platform:
  - linux
testedon: 
  - ee-17.06.2-ee-3
tags:
 - networking
---

## Issue

When deploying an image with `docker-compose` or `docker run` that binds a large port range, `docker-compose` and `docker` commands appear to hang.  Syslog or journalctl may container errors like:

```
Aug 17 10:19:05 trogdor dockerd[2196]: time="2017-08-17T10:19:05.899916777-04:00" level=warning msg="Failed to allocate and map port:  (iptables failed: iptables --wait -t filter -A DOCKER ! -i br-6ed986d59771 -o br-6ed986d59771 -p udp -d 172.19.0.2 --dport 39200 -j ACCEPT:  (fork/exec /usr/sbin/iptables: resource temporarily unavailable)), retry: 1"

Aug 17 10:19:05 trogdor dockerd[2196]: time="2017-08-17T10:19:05.899916777-04:00" level=warning msg="Failed to allocate and map port:  (iptables failed: iptables --wait -t filter -A DOCKER ! -i br-6ed986d59771 -o br-6ed986d59771 -p udp -d 172.19.0.2 --dport 39200 -j ACCEPT:  (fork/exec /usr/sbin/iptables: resource temporarily unavailable)), retry: 1"
```

`docker-compose` may hang with errors like:

```
ERROR: for largeports_porttest_1 UnixHTTPConnectionPool(host='localhost', port=None): Read timed out. (read timeout=60)
ERROR: for porttest UnixHTTPConnectionPool(host='localhost', port=None): Read timed out. (read timeout=60)
ERROR: An HTTP request took too long to complete. Retry with --verbose to obtain debug information. If you encounter this issue regularly because of slow network conditions, consider setting COMPOSE_HTTP_TIMEOUT to a higher value (current value: 60).
```

## Prerequisites

Tested on the following versions, but this issue is not limited to:

- `docker-compose` version 1.15.0
- Docker engine 17.06-ce

## Steps to Reproduce

Using `docker-compose` or `docker run`, create a service that binds a large port range:

1. `docker run --rm -d -p 40000-50000 nginx`
2. The command line interface appears to hang and error messages appear in `journalctl` output like:
    ```
    Failed to allocate and map port: (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 33590 -j DNAT --to-destination 172.17.0.2:49179 ! -i docker0: (fork/exec /usr/sbin/iptables: resource temporarily unavailable))
    ```

## Workaround

Use the `--network=host` option to place this container in the host's network namespace, bypass ingress routing, and prevent hundreds of iptables rules from being added to the host.

> **Note:** Containers in the host network namespace cannot be attached to multiple networks.

It is also unnecessary to publish ports with the `--ports` option because all ports will be exposed through the host's networking stack. For example:

```
docker run --network=host -d nginx
```
