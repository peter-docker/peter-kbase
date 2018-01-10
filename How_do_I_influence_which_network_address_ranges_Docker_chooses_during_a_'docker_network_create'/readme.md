---
type: kbase
version: 3
title: "How do I influence which network address ranges Docker chooses during a 'docker network create'?"
summary: "When the docker network create command is run, it looks for any ranges that appear to be in use and then excludes those ranges from candidate ranges when determining what to use. Docker will see this route and know that the 192.168.1.0/24 range is 'in use' and therefore won't consider it when trying to create new networks. The Docker daemon sees the 172.0.0.0/8 route and, along with the 192.168.1.0/24 route, sees it as 'in use' and will not choose routes that fall in either of those ranges."
dateCreated: "Mon, 01 Aug 2016 21:00:34 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "programmerq"
platform:
  - linux
testedon:
tags:
  - networking
product:
  - EE
---

The following explains how to prevent Docker from creating a network with a specific IP address range.

When the `docker network create` command is run, it looks for any ranges that appear to be in use and then excludes those ranges from candidate ranges when determining what to use. There are a few things taken into consideration when determining if a particular range is "in use" or not. One noteworthy thing is the routing table.

For example, take this fairly basic route from a Linux server:

    $ sudo route -n
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    0.0.0.0         192.168.1.1     0.0.0.0         UG    0      0        0 eth0
    192.168.1.0     0.0.0.0         255.255.255.0   UG    0      0        0 eth0

Let's say that another spot on the network is using the 172.14.65.0/24 range. Right now, anything in that range is routed through the default route, which is 192.168.1.1. This is true for any other IP address range, public or private. My host will pass it through the gateway.

The exception is the 192.168.10/24 range, that explicitly doesn't have a gateway, so it'll be served on the local LAN.

Docker will see this route and know that the 192.168.1.0/24 range is "in use" and therefore won't consider it when trying to create new networks.

You can use this mechanism to our advantage. For example, say you never want Docker to use *any* 172.0.0.0/8 address. You can add a route for 172.0.0.0/8 and configure it so it will behave exactly the same as the default route. Now here's what the routing table looks like:

    $ sudo route -n
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    0.0.0.0         192.168.1.1     0.0.0.0         UG    0      0        0 eth0
    192.168.1.0     0.0.0.0         255.255.255.0   UG    0      0        0 eth0
    172.0.0.0       192.168.1.1     255.0.0.0       UG    0      0        0 eth0

Now, if we try to send traffic to a 172.0.0.0/8 address, it will get sent through the 192.168.1.1 gateway. This is the same behavior as before, but this extra entry gives a hint to the Docker daemon.

The Docker daemon sees the 172.0.0.0/8 route and, along with the 192.168.1.0/24 route, sees it as "in use" and will not choose routes that fall in either of those ranges.
