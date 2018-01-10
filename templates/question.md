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
For the first sentence restate the problem. If necessary, create an introductory paragraph that further explains the problem and include information such as the exact error message or specific product versions. 

The second paragraph should contain the concise, yet detailed answer to the question. When applicable, include links to more information or related kbase articles.
