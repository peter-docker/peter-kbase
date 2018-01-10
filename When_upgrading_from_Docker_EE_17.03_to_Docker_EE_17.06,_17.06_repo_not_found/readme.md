---
type: kbase
version: 3
title: "When upgrading from Docker EE 17.03 to Docker EE 17.06, 17.06 repo not found"
dateCreated: "Fri, 18 Aug 2017 03:45:45 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "tfoxnc"
platform:
testedon:
tags:
  - upgrading
product:
  - EE
---

When upgrading from one major release to another, such as Docker EE 17.03 to Docker EE 17.06, you need to add the new repository and then follow the installation instructions.

## Resolution

To resolve this issue, add the Docker EE 17.06 repo manually. The repo name varies by platform.

1. Go to the Docker Engine installation docs: [https://docs.docker.com/engine/installation/](https://docs.docker.com/engine/installation/ "https://docs.docker.com/engine/installation/")
2. Click on the platform on which you are upgrading Docker EE.
3. Follow the instructions in the **SET UP THE REPOSITORY** section for your platform.
