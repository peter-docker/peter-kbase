---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Replace with descriptive summary
internal: no             # set to yes to keep it internal-only
comment: "internal comments that will only show up in the GH source file"
type: kbase               # set to customerservice if applicable
author:  <docker-id>
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
  - ce         # CE (Docker CE - if equally applicable to both CE and EE, use EE)
  - D4M        # Docker for Mac
  - D4W        # Docker for Windows
  - Hub        # Docker Hub - and all related Store and Cloud functionality while those sites are being deprecated
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.4
platform:           # Optional. Keep all that apply.
  - linux
  - mac
  - windows
  - windows server
  - z systems
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - access control
  - API
  - backup
  - daemon
  - Docker Cloud
  - Docker Content Trust
  - Docker for Azure
  - Docker for AWS
  - error
  - installing
  - logging
  - monitoring
  - networking
  - storage
  - security
  - upgrading
  - uninstalling
---
## Issue

Restate the problem/question, WHY, and HOW it applies to the customer. For example, "If a node fails to join the UCP cluster, the following error will appear in the log files located at <path-to-log-files>:"

```
replace with exact error message
```

## Prerequisites

Before performing these steps, you must meet the following requirements:

- Does this article apply to a specific Docker product version? i.e. Universal Control Plane 1.1.3 and higher, Universal Control Plane 1.0.3 and below
- Does this article apply to a specific OS? a specific version of an OS? i.e. Ubuntu 14.04

## Resolution

Restate the problem and summarize the solution as an introductory sentence or paragraph. For example, "To enable debug logging for the Docker Daemon on Ubuntu 14.04, modifying the Docker daemon configuration file and restarting the daemon as follows:"

1. Step one
2. Step two
    ```
    $ replace with exact command
    ```
3. Step three

## What's Next

Use this OPTIONAL section to guide the reader to further related resources such as a Docker whitepaper, RA, docs page, or other KBase articles.
