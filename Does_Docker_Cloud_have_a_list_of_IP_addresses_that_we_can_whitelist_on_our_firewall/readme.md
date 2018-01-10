---
type: kbase
version: 4
title: "Does Docker Cloud have a list of IP addresses that we can whitelist on our firewall?"
summary: "Question We are trying to set up a firewall that only allow connections to the Docker Cloud service from a specific/range of public IP addresses. Does Docker Cloud have fixed IP addresses or anything like that available? Answer Yes, use the following IP addresses for connections to the Docker Cloud service: 52.204.126.235/32 52.6.30.174/32 52.205.192.142/32 52.205.2.114/32 Please note that you need to keep the following ports open on your host: 6783/tcp 6783/udp 2375/tcp"
dateCreated: "Tue, 02 Aug 2016 02:15:28 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 8
internal: no
author: "kenny.lim"
platform:
testedon:
tags:
product:
  - Hub
---

### Question

We are trying to set up a firewall that only allow connections to the Docker Cloud service from a specific/range of public IP addresses. Does Docker Cloud have fixed IP addresses or anything like that available?

### Answer

Yes, use the following IP addresses for connections to the Docker Cloud service:

    52.204.126.235/32
    52.6.30.174/32
    52.205.192.142/32
    52.205.2.114/32

Please note that you need to keep the following ports open on your host:

* `6783/tcp`
* `6783/udp`
* `2375/tcp`