---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: ucp-auth-store exhibits an unhealthy status even though it appears to be operating normally
internal: no             # set to yes to keep it internal-only
comment: "Tracked via https://github.com/docker/escalation/issues/190"
type: kbase               # set to customerservice if applicable
author:  "squizzi"
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:        
  - ucp-2.2.4
platform:           # Optional. Keep all that apply.
  - linux
  - windows server
  - z systems
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - error
---
## Issue

The `ucp-auth-store` container shows a status of `(unhealthy)` in the output of `docker ps` even though it appears to be operating normally.

## Root Cause

This issue occurs because of a bug in the `enzi` tool which performs the `db-status` command which forms the basis of the health check.

`enzi` is built with the `netgo` tag and is being run in a container created from an `alpine` base image.  This results in a lack of any `/etc/nsswitch.conf` file.  Under these circumstances, the resolver attempts to use DNS before looking at the values defined inside the containers' `/etc/hosts` file.

If the DNS inside the container namespace incorrectly defines `localhost` to anything other then `127.0.0.1`, the `db-status` will fail.

## Resolution

There currently is no resolution for this issue.  The recommendation is to ignore the unhealthy warning as it is incorrect.

This issue will be fixed in a future release of UCP.

Check to see if you are effected by this issue by performing the same underlying health check `ucp-auth-store` is performing, both with `localhost` and `127.0.0.1`.  The following behavior should be observed if the environment is hitting the issue:

```
$ sudo docker exec ucp-auth-store /usr/bin/enzi --debug --db-addr=localhost:12383 db-status -q
DEBU[0000] connecting to db ... 
DEBU[0000] connecting to DB Addrs: [localhost:12383] 
^C

$ sudo docker exec ucp-auth-store /usr/bin/enzi --debug --db-addr=127.0.0.1:12383 db-status -q
DEBU[0000] connecting to db ... 
DEBU[0000] connecting to DB Addrs: [127.0.0.1:12383] 
DEBU[0000] getting server status... 
DEBU[0000] getting table status... 
OK 
```

If you are affected by this issue, you will receive an `OK` status from the health check when `127.0.0.1` is used.  `localhost` will simply times out.


