---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to test the dtr-ol network
internal: no             # set to yes to keep it internal-only
comment: ""
type: kbase               # set to customerservice if applicable
author:  quaddo
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.3
platform:           # Optional. Keep all that apply.
  - linux
  - windows server
  - z systems
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - networking
---
## Issue

By default, DTR replicas use the `dtr-ol` overlay network to communicate with each other. This article provides steps to validate the functionality of this network if you suspect a loss of functionality somewhere on the `dtr-ol` overlay network and are interested in identifying the scope of loss of functionality.

## Prerequisites

Before performing these steps, you must meet the following requirements:

- DTR configured to run with at least 3 replicas

## Resolution

As the `dtr-ol` overlay network is created and managed by the manager nodes, it can be accessed and used by any node in the cluster, under normal circumstances.

The source/destination container instructions provided below are to be used to validate `dtr-ol` between any two nodes in the cluster. While these steps mainly focus on validating the health between the DTR nodes, there may be a need or want to validate between other nodes.

What follows below are a few likely test scenarios to try.

### Between two DTR nodes, testing a DTR container

1. Start a test container named <^>dtrol-test1<^^> on one DTR node:

  ```
  $ docker run --rm -it --net dtr-ol --name <^>dtrol-test1<^^> alpine sh
  / #
  ```

2. On a different DTR node, identify one of the DTR containers you would like to try to ping:

  ```
  $ docker ps -f name=dtr-registry
  CONTAINER ID        IMAGE                       COMMAND             CREATED             STATUS              PORTS                       NAMES
  e80028d0c15e        docker/dtr-registry:2.3.3   "/bin/registry"     6 days ago          Up 2 days           80/tcp, 443/tcp, 3000/tcp   <^>dtr-registry-4b92d1271f70<^^>
  ```

  In the above example, the target name is <^>`dtr-registry-4b92d1271f70`<^^>.

3. Back on the first node, ping the selected container:

  ```
  $ docker run --rm -it --net dtr-ol --name <^>dtrol-test1<^^> alpine sh
  / # ping -c 5 <^>dtr-registry-4b92d1271f70<^^>
  PING <^>dtr-registry-4b92d1271f70<^^> (10.1.0.8): 56 data bytes
  64 bytes from 10.1.0.8: seq=0 ttl=64 time=1.010 ms
  64 bytes from 10.1.0.8: seq=1 ttl=64 time=0.960 ms
  64 bytes from 10.1.0.8: seq=2 ttl=64 time=1.035 ms
  64 bytes from 10.1.0.8: seq=3 ttl=64 time=0.866 ms
  64 bytes from 10.1.0.8: seq=4 ttl=64 time=0.927 ms

  --- <^>dtr-registry-4b92d1271f70<^^> ping statistics ---
  5 packets transmitted, 5 packets received, 0% packet loss
  round-trip min/avg/max = 0.866/0.959/1.035 ms
  / #
  ```

  Success.

4. Execute the ping test in the reverse direction.

  First we exec a shell on the container, then proceed with the ping test.

  ```
  $  docker exec -it <^>dtr-registry-4b92d1271f70<^^> sh
  / # ping -c 5 <^>dtrol-test1<^^>
  PING dtrol-test1 (10.1.0.21): 56 data bytes
  64 bytes from 10.1.0.21: seq=0 ttl=64 time=0.971 ms
  64 bytes from 10.1.0.21: seq=1 ttl=64 time=0.950 ms
  64 bytes from 10.1.0.21: seq=2 ttl=64 time=1.045 ms
  64 bytes from 10.1.0.21: seq=3 ttl=64 time=1.673 ms
  64 bytes from 10.1.0.21: seq=4 ttl=64 time=1.523 ms

  --- <^>dtrol-test1<^^> ping statistics ---
  5 packets transmitted, 5 packets received, 0% packet loss
  round-trip min/avg/max = 0.950/1.232/1.673 ms
  / #
  ```

  Success again. The `dtr-ol` overlay network appears healthy, at least between these two nodes.

  If required, you can repeat the above tests against another DTR container on any DTR replica.

_Notes:_

- The name <^>dtrol-test1<^^> is completely arbitrary. You can use any name here which is compliant with the Docker CLI and unlikely to conflict with any other container name in the cluster.
- This is a non-destructive test.
- If the `alpine` container is unavailable to your particular cluster (eg, for security reasons), you may use any other container which has a working `/bin/ping` in it.

### Between two nodes, without testing DTR containers

There may be times that you want to validate the `dtr-ol` overlay network without needing to check the reachability of a specific DTR container. The following test can be done between any two nodes, even DTR nodes.

1. Start a test container on one node:

  ```
  $ docker run --rm -it --net dtr-ol --name <^>dtrol-test1<^^> alpine sh
  / #
  ```

2. Start a test container on another node:

  ```
  $ docker run --rm -it --net dtr-ol --name <^>dtrol-test2<^^> alpine sh
  / #
  ```

  _Note the different container names used, above._

3. From the second container, ping the first container:

  ```
  / # ping -c 5 <^>dtrol-test1<^^>
  PING <^>dtrol-test1<^^> (10.1.0.21): 56 data bytes
  64 bytes from 10.1.0.21: seq=0 ttl=64 time=1.230 ms
  64 bytes from 10.1.0.21: seq=1 ttl=64 time=0.983 ms
  64 bytes from 10.1.0.21: seq=2 ttl=64 time=0.922 ms
  64 bytes from 10.1.0.21: seq=3 ttl=64 time=0.970 ms
  64 bytes from 10.1.0.21: seq=4 ttl=64 time=0.909 ms

  --- <^>dtrol-test1<^^> ping statistics ---
  5 packets transmitted, 5 packets received, 0% packet loss
  round-trip min/avg/max = 0.909/1.002/1.230 ms
  / #
  ```

4. From the first container, ping the second container:

  ```
  / # ping -c 5 <^>dtrol-test2<^^>
  PING <^>dtrol-test2<^^> (10.1.0.22): 56 data bytes
  64 bytes from 10.1.0.22: seq=0 ttl=64 time=0.899 ms
  64 bytes from 10.1.0.22: seq=1 ttl=64 time=0.947 ms
  64 bytes from 10.1.0.22: seq=2 ttl=64 time=0.953 ms
  64 bytes from 10.1.0.22: seq=3 ttl=64 time=0.928 ms
  64 bytes from 10.1.0.22: seq=4 ttl=64 time=1.016 ms

  --- <^>dtrol-test2<^^> ping statistics ---
  5 packets transmitted, 5 packets received, 0% packet loss
  round-trip min/avg/max = 0.899/0.948/1.016 ms
  / #
  ```

  Both of the containers above have successfully pinged one another. The `dtr-ol` overlay network appears healthy between these two nodes.


## What's Next

If you have reason to suspect that your `dtr-ol` network isn't working as expected after having followed the above instructions, [please open a support ticket](https://support.docker.com/), being sure to include your test results from the above.
