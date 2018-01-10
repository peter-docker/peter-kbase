---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Why won't my container run on a manager or replica?
internal: no             # set to yes to keep it internal-only
comment: "internal comments that will only show up in the GH source file"
type: kbase               # set to customerservice if applicable
author:  bryceryan-docker
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.06.2-ee-3
  - ucp-2.2.3
platform:           # Optional. Keep all that apply.
  - linux
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - access control
  - error
  - monitoring
---

## Issue

After setting up and configuring a cluster for Docker Enterprise Edition (EE), I've created a new service with a constraint 
to run the tasks in that service on a UCP Manager Node or a DTR replica node. The service starts successfully, but when I
inspected the service, I see it running on another node. Why is this happening?

## Resolution

This behavior is the default, by-design function of UCP. A best practice and Docker recommendation is to reserve the nodes
running UCP and DTR to execute only containers directly related to providing UCP and DTR services. Consequently, the default,
out of the box, configuration for UCP-based clusters prevents user-originated or non-system containers from running on
nodes running UCP or DTR.

Docker does not recommend changing this configuration unless you do so strictly for testing purposes (and intend to change
this back prior to running production loads), unless advised to do so by Docker Technical Support or Technical Field
personnel, or unless you understand the risks inherent in running workloads on the cluster's "master" nodes. If you do
wish to run workloads on the UCP or DTR nodes, please see the following document, or the documentation for your release
of UCP: 
[https://docs.docker.com/datacenter/ucp/2.2/guides/admin/configure/restrict-services-to-worker-nodes/](https://docs.docker.com/datacenter/ucp/2.2/guides/admin/configure/restrict-services-to-worker-nodes/)
