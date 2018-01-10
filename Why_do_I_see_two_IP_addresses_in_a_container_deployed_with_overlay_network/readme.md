---
type: kbase
version: 4
title: "Why do I see two IP addresses in a container deployed with overlay network?"
summary: "The first network interface eth0 (IP: 172.16.16.2) is because of the Overlay Network named overlaytest, and the second network interface eth1(IP: 172.18.0.2) is because of the Bridge Network named docker_gwbridge. Overlay Network uses the first network to discover nodes among the network and the second one (Bridge Network) to connect to the host."
dateCreated: "Tue, 14 Jun 2016 18:27:11 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "sabin.basyal"
platform:
  - linux
testedon:
tags:
  - networking
product:
  - EE
---

A container deployed with Overlay Network will always have 2 IP addresses.

Overlay Network uses two networks — one for overlay and one for bridge.

For example, for a container using Overlay Network created with `--subnet=172.16.16.0/24 --gateway=172.16.16.1`, the `ifconfig` output is as follows:

    eth0      Link encap:Ethernet  HWaddr 02:42:ac:10:10:02
              inet addr:172.16.16.2  Bcast:0.0.0.0  Mask:255.255.255.0
              inet6 addr: fe80::42:acff:fe10:1002/64 Scope:Link
              UP BROADCAST RUNNING MULTICAST  MTU:1450  Metric:1
              RX packets:10 errors:0 dropped:0 overruns:0 frame:0
              TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:0
              RX bytes:788 (788.0 B)  TX bytes:648 (648.0 B)
    
    eth1      Link encap:Ethernet  HWaddr 02:42:ac:12:00:02
              inet addr:172.18.0.2  Bcast:0.0.0.0  Mask:255.255.0.0
              inet6 addr: fe80::42:acff:fe12:2/64 Scope:Link
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:25618 errors:0 dropped:0 overruns:0 frame:0
              TX packets:12415 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:0
              RX bytes:38385409 (38.3 MB)  TX bytes:827780 (827.7 KB)
    
    lo        Link encap:Local Loopback
              inet addr:127.0.0.1  Mask:255.0.0.0
              inet6 addr: ::1/128 Scope:Host
              UP LOOPBACK RUNNING  MTU:65536  Metric:1
              RX packets:18 errors:0 dropped:0 overruns:0 frame:0
              TX packets:18 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:0
              RX bytes:1629 (1.6 KB)  TX bytes:1629 (1.6 KB)

Executing `docker network ls` shows the following:

    NETWORK ID          NAME                DRIVER
    2d50f3855a02        overlaytest         overlay
    e44d84116ac3        dtr-ol              overlay
    3b344ef7b5ee        bridge              bridge
    f18d6736bf38        none                null
    7e3fdb4de032        host                host
    495ece1028c1        docker_gwbridge     bridge

The first network interface `eth0` (IP: `172.16.16.2`) is because of the Overlay Network named **overlaytest**, and the second network interface `eth1`(IP: `172.18.0.2`) is because of the Bridge Network named **docker_gwbridge**.

Overlay Network uses the first network to discover nodes among the network and the second one (Bridge Network) to connect to the host. They are also sometimes referred to as the `east/west` network and the `north/``south`network.
