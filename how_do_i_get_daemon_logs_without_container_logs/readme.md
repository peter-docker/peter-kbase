---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How do I get the Docker daemon logs without all the container updates?
internal: no             # set to yes to keep it internal-only
comment: ""
type: kbase               # set to customerservice if applicable
author:  bryceryan-docker
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ce-17.10.0-ce
platform:           # Optional. Keep all that apply.
  - linux
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - logging
  - daemon
---
## Issue

Docker Technical Support, and customer operations teams, may need to review what the Docker Engine (aka the Docker Daemon)
is doing, or has done, and may request a copy of the daemon logs. For swarms or clusters that are very busy, and performing
a lot of work, these logs also include many log entries from each application container or task that has been run
in the swarm or cluster. These application container entries may swamp or outnumber the engine actions and log entries,
making it more difficult to troubleshoot the problem.

## Prerequisites

Before performing these steps, you must meet the following requirements:

- A Docker Engine greater than 1.12
- A node OS that has the `journalctl` and `jq` commands installed

## Resolution

To list the Docker Daemon logs and exclude any container-specific log entries, run a command similar to the following
on any node where you want to retrieve and filter the docker daemon logs:

```
journalctl -o json -u docker | jq -c -M -r 'select(has("CONTAINER_ID") | not)' | jq -c -M -r '.MESSAGE'
```


