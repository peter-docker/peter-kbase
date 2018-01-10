---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: UCP Scheduler restrictions not being honored
internal: no       
comment: ""
type: kbase              
author:  lmuchnick
product:
  - ee 
testedon:
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.4
platform:
  - linux
  - windows server
tags:
  - access control
  - security
---

## Issue

Within the UCP UI admin settings under **Scheduler**, there is options to allow admins and/or users to schedule on DTR and UCP nodes. 

![ucp_sched_opt](./images/UCP_sched.png)

With these options unchecked, admins and/or users are restricted from deploying to UCP Managers and DTR Nodes — they can only schedule on worker nodes.

These settings are not being honored.

## Resolution

If these settings are not being honored, it is most likely because you are connecting to a UCP manager node directly through SSH without a [client bundle](https://docs.docker.com/datacenter/ucp/2.2/guides/user/access-ucp/cli-based-access/#download-client-certificates). Running the stack deploy directly on the UCP manager node bypasses the UCP controller container, and, therefore, the container scheduling options set in the UCP UI are not honored. 

The client bundle deploys the stack from within the UCP controller container, and the scheduling options set in the UI are honored.

> **Note:** Running the stack deploy from within the UCP UI also honors the scheduling options.
