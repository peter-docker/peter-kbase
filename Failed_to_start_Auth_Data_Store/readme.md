---
type: kbase
version: 6
title: "Failed to start Auth Data Store"
summary: "Overview I receive this error during a clean installation of UCP: Failed to start Auth Data Store. Run 'docker logs ucp-auth-store' for more details. Resolution There's a known issue originating from Docker Engine 1.11 where the hostname of a UCP node cannot be more than 48 characters long. To resolve this issue, shorten your domain name to make it 48 characters or less. Note: This issue is only valid with UCP 1.1.1 and Docker Engine 1.11"
dateCreated: "Mon, 11 Jul 2016 23:27:06 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "sabin.basyal"
platform:
testedon:
  - cse-1.11.1-cs1
  - ucp-1.1.1
tags:
  - installing
product:
  - EE
---

### Overview

> **Note**: This issue is only valid with **UCP 1.1.1** and **Docker Engine 1.11**

I receive this error during a clean installation of UCP:

    Failed to start Auth Data Store. Run "docker logs ucp-auth-store" for more details.

### Resolution

There's a known issue originating from Docker Engine 1.11 where the hostname of a UCP node cannot be more than **48 characters** long.

To resolve this issue, shorten your domain name to make it 48 characters or less.
