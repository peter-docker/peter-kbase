---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Working with Docker Certified Plugin Cases
internal: yes             # set to yes to keep it internal-only
comment: "internal comments that will only show up in the GH source file"
type: kbase               # set to customerservice if applicable
author:  sixcounts
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
platform:           # Optional. Keep all that apply.
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - error
  - installing
  - storage
  - upgrading
---
## Issue

Customers working with Docker Certified Plugins download from store.docker.com and are experiencing problems with plugins not working correctly.

## Prerequisites

- Installing or upgrading a Docker Certified Plugin 
- Docker EE 


## Resolution

Workflow to help customers working with Docker Certified Plugins.

1. Review any information provided to you within the Support Case related to what plug-in the customer is using.
2. Check out the plugin on [Docker’s Store](https://store.docker.com)
  - once there you can ether manually click on the the plug-ins icon or do a search for the specific plug-in the customer is referring to.
3. Once viewing the Docker Certified Plug-In, make sure to get familiar with all tabs for:
  - Description
  - Reviews
  - Resources
4. The Resources tab will provide you links for both internal support for the plug-in as well as any internal documentation surrounding it.
5. If the Support Case requires opening a GitHub issue follow the step above to get to the plug-ins internal support page, and create a new issue from there.
6. Reach out to the vendor if necessary following the process on the [TSANet Support process page](https://docker.atlassian.net/wiki/spaces/DSS/pages/132668073/TSANet+Support+Process)
7. Once communicating with both the vendor and Docker internal support for the specific plug-in and pull request might be needed to apply a fix into the next engine releases.
8. Document all information internally in the Support Case and provide meaningful updates to customer, if a pull request is need to resolve the issue you can check in with the pull request's assignee to check the current status.
9. Continue to have open communication with the customer around the current status of the support case and actions being taken to resolve their issue.
