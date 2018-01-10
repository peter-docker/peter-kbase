---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Microsoft Support for EE for Windows Server
internal: yes             # set to yes to keep it internal-only
comment: "internal comments that will only show up in the GH source file"
type: kbase               # set to customerservice if applicable
author:  sixcounts
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
platform:           # Optional. Keep all that apply.
  - windows server
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - installing
  - upgrading
  - uninstalling
---
For issues related to Windows Server running EE the support workflow should be for the customer to engage with Microsoft for L1 and L2 support. If after troubleshooting and working with Microsoft Support they determine that Docker Support is needed for L3 support then a Docker Support Case is opened for Windows Server for EE for the product listing and L3 for the category listing. The Docker Support case will then be assigned to a member of the Docker Support Windows Team to continue working with the customer on the issue and Microsoft.

