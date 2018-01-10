---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Error Invalid license when uploading a new license to the UCP UI
internal: no             
comment: 
type: kbase               
author:  hannahagee
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
platform:           
tags:               
  - installing
---
## Issue

When uploading a new license to the UCP UI, the following pop up message is shown:

```
License
Error: Invalid license
```

## Prerequisites

Relevant for all UCP versions.

## Resolution

To begin troubleshooting this error:

1. Compare the number of current nodes with the number of nodes allowed for currently applied license. If there are already more nodes than allowed on the new license, it will fail upon uploading with `Error: Invalid license`.
2. Provide a copy of the license to support to validate. Support can also upload the license to a test environment for validation.

