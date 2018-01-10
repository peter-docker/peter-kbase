# Metadata

All articles must contain a [YAML](https://en.wikipedia.org/wiki/YAML) metadata section starting at line 1 of the `readme.md` file. The [template files](../templates/) contain YAML frontmatter specific to the type of document. Please use the templates to determine which variables are required for different document types. 

**This is only a reference for all possible variables in all doc types.**


```
---
type: architecture  # Type of article: architecture (Reference Architectures), guide (Solution Guides), kbase (kbase articles using any template), customerservice (non-technical articles), policy (policy docs)
title: This is the article title   #required
internal: no | yes  # if not yes, then optional
version: 1          # Not used for kbase articles. Use for RAs and Solution Guides. Will appear in the PDF versions.
comment: "internal comments that will only show up in the GH source file i.e. eng ticket numbers, case numbers"
author:             # The author's Docker ID. One or more. Used as key for author bio page and in the attribution line.
  - "beyonce.knowles"
summary:           # Short description used to preview the document in various views. Should be <200 characters. Not used for kbase articles.
product: 
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
  - ce         # CE (Docker CE - if equally applicable to both CE and EE, use EE)
  - D4M        # Docker for Mac
  - D4W        # Docker for Windows
  - Hub        # Docker Hub - and all related Store and Cloud functionality while those sites are being deprecated
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.06.2-ee-3
  - ucp-2.2.0
  - dtr-2.3.0
platform:
  - linux
  - mac
  - windows
  - windows server
  - z systems
tags:               # List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags. Used for related articles, search, etc. Optional.
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
callout:            # Optional: Call to action that will be placed adjacent to the content.
  - text: "Try Docker EE for free."
  - buttonlabel: "Try Now!"
  - url: http://www.dockertrial.com/
---
```

All data above is for example only. Valid values are described in the in-line comments.
