---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Error when reconfiguring DTR to use NFS
internal: yes             # set to yes to keep it internal-only
comment: "internal comments that will only show up in the GH source file"
type: kbase               # set to customerservice if applicable
author:  darwintraver
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.4
platform:           # Optional. Keep all that apply.
  - linux
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - error
  - installing
  - storage
---
## Issue

During DTR reconfiguring with NFS, the following error message can occur:

```
Failed to verify NFS: Error response from daemon: {"message":"could not find any nodes on which a volume could be created"} 
```

## Prerequisites

Before performing these steps, you must meet the following requirements:

- UCP 2.2.x
- DTR 2.2.x/2.3.x


## Resolution

UCP scheduler options can block deployment of the DTR NFS volume to certain swarm nodes.

1. Log into the UCP GUI.
2. Browse to **Admin Settings** —> **Scheduler**.
3. Confirm the following setting is enabled:
    ```
    Allow Administrators To Deploy Containers On UCP Managers Or Nodes Running DTR
    ```

> **Note:** It is not best practice to add workloads to UCP or DTR managers. Only do so if absolutely necessary.

## What's Next

Documentation reference: [https://docs.docker.com/datacenter/dtr/2.3/guides/admin/configure/external-storage/nfs/](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/configure/external-storage/nfs/)
