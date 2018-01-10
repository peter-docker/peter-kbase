---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Moving DTR object storage
internal: no             # set to yes to keep it internal-only
comment: "internal comments that will only show up in the GH source file"
type: kbase               # set to customerservice if applicable
author:  hannahagee
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
platform:           # Optional. Keep all that apply.
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - storage
---

When moving DTR object storage, considering the following:

1. Make a copy from the source to the destination. Identical file structure/hierarchy no matter which object storage system chosen. Important to note the command/tool should make an identical copy of the source. 
2. Once the image data has been copied over you will need to change the DTR storage settings to reference the new backend store.

Please note, as of now, Docker does not have an officially sanctioned tool for copying this data.
