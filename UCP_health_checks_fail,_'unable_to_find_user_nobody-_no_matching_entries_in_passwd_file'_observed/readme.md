---
type: kbase
version: 3
title: "UCP health checks fail, 'unable to find user nobody: no matching entries in passwd file' observed"
summary: "Suddenly UCP health checks begin to fail on nodes, and a docker inspect of nodes which appear to show an 'unhealthy' status in docker ps output reports an exit status of: 'Health': { 'Status': 'unhealthy', 'FailingStreak': 812, 'Log': [ { 'Start': '2017-07-05T15:50:57.765421798-04:00', 'End': '2017-07-05T15:50:57.766049318-04:00', 'ExitCode': -1, 'Output': 'unable to find user nobody: no matching entries in passwd file' }, unable to find user nobody: no matching entries in passwd file"
dateCreated: "Thu, 06 Jul 2017 18:39:39 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "squizzi"
platform:
  - linux
testedon:
  - ucp-2.1.4
tags:
product:
  - EE
---

## Issue

* Suddenly UCP health checks begin to fail on nodes, and a `docker inspect` of nodes which appear to show an 'unhealthy' status in `docker ps` output reports an exit status of:

    "Health": {
                    "Status": "unhealthy",
                    "FailingStreak": 812,
                    "Log": [
                        {
                            "Start": "2017-07-05T15:50:57.765421798-04:00",
                            "End": "2017-07-05T15:50:57.766049318-04:00",
                            "ExitCode": -1,
                            "Output": "unable to find user nobody: no matching entries in passwd file"
                        },

* When attempting to run `docker exec` commands, the following error is observed:

    unable to find user nobody: no matching entries in passwd file

* The UCP cluster appears to otherwise be healthy, even though health checks seemingly fail.

# Prerequisites

This issue has been observed on the following versions:

* Universal Control Plane: 2.1.4
* Engine: 17.03.1-ee

# Steps

Currently there is no resolution to this issue. It is being tracked via an internal bug.

## Workaround

Restart the Docker Engine to temporarily resolve this issue:

    $ sudo systemctl restart docker

The health checks should begin to succeed again, and the cluster should report a healthy status.