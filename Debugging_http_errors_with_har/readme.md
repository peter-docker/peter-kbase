---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Debugging HTTP errors with HAR
internal: no             
comment: ""
type: kbase             
author:  romainbelorgey
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
  - ce         # CE (Docker CE - if equally applicable to both CE and EE, use EE)
  - D4M        # Docker for Mac
  - D4W        # Docker for Windows
  - Hub        # Docker Hub - and all related Store and Cloud functionality while those sites are being deprecated
testedon:
platform:
  - linux
  - mac
  - windows
  - windows server
  - z systems
tags:               
  - daemon
  - networking
  - error
---
## Issue

For the UCP UI or a web application running on a Docker container, when you get a request error, it can be difficult to understand what the problem is or how to share it with support. Saving a HAR (HTTP Archive) file allows you to share the requests and the request responses to a file that can then be analyzed by a variety of tools.

"One of the goals of the HTTP Archive format is to be flexible enough so, it can be adopted across projects and various tools. This should allow effective processing and analyzing data coming from various sources. Notice that resulting HAR file can contain privacy & security sensitive data and user-agents should find some way to notify the user of this fact before they transfer the file to anyone else."

For details, check out the [HAR specifications website](http://www.softwareishard.com/blog/har-12-spec/).

## Saving the HAR File

Every browser permits you to see network requests. For example:

- Firefox
    - Go to **Web Developer** -> **Network**.
    - Right click on the data and select **Save all as Har**.
- Chrome
    - Go to **Developer tools**.
    - Click on the **Network** tab.
    - Right click on the data and select **Save as Har with content**.

You can also check on the [HAR archive website](http://www.softwareishard.com/blog/har-adopters/) for a list of programs available.

## Viewing the HAR File

Many options exist to view the data in the HAR file including:

- Online tool: [http://www.softwareishard.com/har/viewer/](http://www.softwareishard.com/har/viewer/)
- CLI tool: [https://github.com/jarib/har](https://github.com/jarib/har)
- Additional tools: [http://www.softwareishard.com/blog/har-viewer/](http://www.softwareishard.com/blog/har-viewer/)

