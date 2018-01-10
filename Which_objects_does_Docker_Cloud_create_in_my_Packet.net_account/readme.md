---
type: kbase
version: 3
title: "Which objects does Docker Cloud create in my Packet.net account?"
summary: "Which objects does Docker Cloud create in my Packet.net account? Docker Cloud creates a project named “Docker Cloud” which contains all the devices that Docker Cloud deploys, no matter what type of device you chose. Type 1 devices have a RAID 1 of two SSD drives mounted in “/” Type 3 devices also have a RAID 1 of two SSD drives mounted in “/”, and also offers two NVMe drives without being mounted. Docker Cloud mounts a RAID 1 in “/var/lib/docker”."
dateCreated: "Thu, 08 Oct 2015 19:11:24 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "justinnevill"
platform:
testedon:
tags:
  - Docker Cloud
product:
  - Hub
---

## Question

Which objects does Docker Cloud create in my Packet.net account?

## Answer

Docker Cloud creates a project named **Docker Cloud** which contains all the devices that Docker Cloud deploys, no matter what type of device you chose.

Device storage is organized the following way:

* Type 1 devices have a RAID 1 of two SSD drives mounted in `/`.
* Type 3 devices also have a RAID 1 of two SSD drives mounted in `/` and also offers two NVMe drives without being mounted. Docker Cloud mounts a RAID 1 in `/var/lib/docker`.

An SSH keypair named `dockercloud-<uuid>` is created if no key is found in your account.
