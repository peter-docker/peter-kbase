---
type: kbase
version: 3
title: "On DTR 2.2.1, why does the dtr-api container steadily use memory until DTR is unavailable?"
summary: "On DTR 2.2.1, the dtr-api container has been known to steadily use memory until DTR is unavailable, even if the cluster is not busy. This has been resolved in DTR 2.2.3. Upgrading to DTR 2.2.3 will fix this issue. For details on upgrading DTR, refer to the following: https://docs.docker.com/datacenter/dtr/2.2/guides/admin/upgrade/"
dateCreated: "Tue, 28 Mar 2017 12:46:08 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
testedon:
  - dtr-2.2.1
tags:
product:
  - EE
---

On DTR 2.2.1, the `dtr-api`container has been known to steadily use memory until DTR is unavailable, even if the cluster is not busy.

This has been resolved in DTR 2.2.3. Upgrading to DTR 2.2.3 will fix this issue. For details on upgrading DTR, refer to the following:

<https://docs.docker.com/datacenter/dtr/2.2/guides/admin/upgrade/>