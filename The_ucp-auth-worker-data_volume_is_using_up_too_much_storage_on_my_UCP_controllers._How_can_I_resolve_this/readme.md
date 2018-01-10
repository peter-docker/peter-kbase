---
type: kbase
version: 5
title: "The ucp-auth-worker-data volume is using up too much storage on my UCP controllers. How can I resolve this?"
summary: "The logs can become an issue with storage if a 'Debug' severity level is used, or if there is already insufficient storage space available for Docker volumes prior to installing UCP. The following method can be used to resolve issues with insufficient space for the ucp-auth-worker-data volume: Modify the severity level to be less aggressive in logging via the UCP dashboard (Admin Settings > Logs > Log Severity Level) or set up a remote log server as described in the documentation."
dateCreated: "Mon, 06 Feb 2017 09:01:25 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "yiming"
platform:
testedon:
tags:
  - storage
product:
  - EE
---

**The ucp-auth-worker-data volume can store a huge amount of logging data**. By default, up to 500 job logs are stored, and log severity is set to "Info" by default. The logs can become an issue with storage if a "Debug" severity level is used, or if there is already insufficient storage space available for Docker volumes prior to installing UCP.

The following method can be used to resolve issues with insufficient space for the ucp-auth-worker-data volume:

1. Stop the ucp-auth-worker container.
2. Remove the logs in `/var/lib/docker/volumes/ucp-auth-worker-data/_data/jobs` (but do not attempt to remove the ucp-auth-worker-data volume).
3. Restart the ucp-auth-worker container.
4. Modify the severity level to be less aggressive in logging via the UCP dashboard (**Admin Settings** > **Logs** > **Log Severity Level**) or set up a remote log server as described in the [documentation](https://docs.docker.com/datacenter/ucp/2.0/guides/configuration/configure-logs/ "https://docs.docker.com/datacenter/ucp/2.0/guides/configuration/configure-logs/").
