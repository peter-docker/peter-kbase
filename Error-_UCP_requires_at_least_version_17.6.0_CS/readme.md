---
type: kbase
version: 3
title: "Error: UCP requires at least version 17.6.0 CS"
summary: "Upgrade Error Message: While attempting to upgrade an older version of UCP, the upgrade fails and the following error message is generated: In mid- to later-2017, Docker introduced Docker Enterprise Edition (Docker EE) and also changed the version numbers to the form YY.MM (last two digits of the calendar year, followed by the month number), followed by ee or EE. To resolve this error, locate and install an appropriate EE version of the Engine, and retry the UCP upgrade."
dateCreated: "Wed, 20 Sep 2017 15:48:46 GMT"
dateModified: "Wed, 04 Oct 2017 03:43:49 GMT"
dateModified_unix: 1507088629
upvote: 0
internal: no
author: bryceryan-docker
platform:
testedon:
  - cse-1.13.1-cs1
tags:
  - upgrading
  - error
product:
  - EE
---

**Upgrade Error Message:** While attempting to upgrade an older version of UCP, the upgrade fails and the following error message is generated:

    Your engine version 1.13.1 CS is too old. UCP requires at least version 17.6.0 CS or 17.6.0 OSS.

Where can you find the 17.6.0 CS engine?

## Prerequisites

One sees this error when upgrading UCP from an earlier version, and you're currently using Docker Engine version 1.13.x CS.

## Resolution

**Background:** The error message is incorrect and misleading. There is no Docker Engine version 17.6.0 CS or later. "CS" stands for "Commercial Support", and that release branch was retired with Engine 1.13.x. In mid- to later-2017, Docker introduced Docker Enterprise Edition (Docker EE) and also changed the version numbers to the form `YY.MM` (last two digits of the calendar year, followed by the month number), followed by `ee` or `EE`. Thus, this error message actually means that the update requires Engine version 17.06.0 EE or later. To resolve this error, locate and install an appropriate EE version of the Engine, and retry the UCP upgrade.
