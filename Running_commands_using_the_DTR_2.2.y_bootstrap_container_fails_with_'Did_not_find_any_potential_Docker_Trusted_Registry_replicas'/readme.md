---
type: kbase
version: 3
title: "Running commands using the DTR 2.2.y bootstrap container fails with 'Did not find any potential Docker Trusted Registry replicas'"
summary: "This issue should not occur when utilizing the DTR version 2.3.0 bootstrapper to run the above commands. A recent change within UCP triggered dtr-* containers to be hidden as system containers, which triggers an issue inside the bootstrapper code as it relies on seeing the running containers to decide how to make changes. Use the now functional DTR bootstrapper operations to upgrade to DTR 2.3.0 then upgrade to UCP 2.2.0."
dateCreated: "Fri, 18 Aug 2017 15:01:26 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "squizzi"
platform:
testedon:
tags:
product:
  - EE
---

When utilizing the DTR bootstrapper commands, for example:

* backup
* restore
* reconfigure
* remove
* join
* upgrade

    docker run it --rm docker/dtr 
    COMMAND

on an existing DTR 2.2.y environment where UCP 2.2.0 is running, the following error occurs:

    ERRO[0001] Did not find any potential Docker Trusted Registry replicas.

## Prerequisites

* This issue is specific to environments running: 
    * UCP 2.2.0
    * DTR 2.2.y
* This issue should not occur when utilizing the DTR version 2.3.0 bootstrapper to run the above commands.

## Root Cause

The DTR bootstrapper relies heavily on the use of `docker ps`. A recent change within UCP triggered `dtr-*` containers to be hidden as system containers, which triggers an issue inside the bootstrapper code as it relies on seeing the running containers to decide how to make changes.

## Resolution

You can try to use the `--existing-replica-id` flag to workaround this issue. This flag will allow you to provide the existing replica-id of the replica you are trying to work with. You can find the existing replica id by viewing the output of `docker ps` on the host:

    docker ps --filter name=dtr | awk '{print $NF}' | sed 's/dtr-\(.*\)-//g' | grep -vi names | head -n1

However, in some cases, even with the `--existing-replica-id` flag in place, bootstrapper commands will fail. If this occurs, their are two recommendations which can be made:

1. If DTR 2.3.0 is required: Restore UCP backups to a point in the environment pre-upgrade, where UCP 2.1.5 was installed. Use the now functional DTR bootstrapper operations to upgrade to DTR 2.3.0 then upgrade to UCP 2.2.0.

2. Or, wait for DTR 2.2.8 to be released which will resolve the issues of bootstrapper commands failing to function.