---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to get help with your Windows container issues
internal: no       # if not yes, then optional
comment: none
type: kbase
author: "eiichik"
product: 
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
  - ce         # CE (Docker CE - if equally applicable to both CE and EE, use EE)
  - D4W        # Docker for Windows
platform:
  - windows
  - windows server
tags:              
---

## Issue
  
Docker Engine on Windows platform was developed by Microsoft with partnership from Docker Inc.  Because of this, support must be provided by Microsoft, Docker, or Docker Community depending on the component and edition used.  This article shows you how to find the right resource.
 
## Community Edition vs. Enterprise Edition

[Docker Community Edition (CE)](https://www.docker.com/community-edition) is free to use and great for small teams getting started with Docker and container-based applications. Docker CE does not have commercial support. However, there is a Docker community and forums to help with Docker CE.

[Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition) is a subscripton-based container platform for building and managing applications from development to production.
 
You can check which edition is installed by running following command:
```
PS > docker version
```
The output should state *ce* or *ee*. For example:
```
Server:
 Version:      17.09.0-<^>ce<^^>
``` 
 
## Who to Ask for Help

The following table should direct you to the right resource for your Windows platform issues:

| Name                                        | Available On | Intended Use                                          | Where to get help                                                               |
|----------------------------------------     |----- |---------------------------------------------------    | -----------------------------------------------------------------------------    |
| Docker EE Basic on Windows Server 2016   | Windows Server 2016 | Running test on stand alone server     | [Microsoft Premier Support](https://support.microsoft.com/en-us/premier)     | 
| Docker EE Standard<br/><br/>Docker EE Advanced| Windows Server 2016 | Running QA, staging, or production on a cluster| Windows engine issues: <br/> [Microsoft Premier Support](https://support.microsoft.com/en-us/premier) <br/><br/> UCP or DTR component issues: <br/>[Docker Enterprise Support](https://support.docker.com/)
| Docker CE Desktop for Windows                 |  Windows 10 | Developing container images on a desktop              | [Docker Community Forums](https://forums.docker.com/) <br/> [Third-Party Communities](http://www.docker.com/community/) <br/> [Docker Documentation](https://docs.docker.com/) <br/> [Docker Training](https://training.docker.com/)                         |          |
### Scope for Microsoft Premier Support
 
If you can reproduce the error even when the node is removed from UCP cluster, Microsoft support can assist. 
 
Examples:
  - Running specific `docker run` or `docker service create` fails
  - Docker service on a host machine fails with HNS or Hyper-V related error on Windows event log
  - Questions on customizing Windows host
 
_Microsoft support team will escalate the issue to Docker if required._
 
### Scope for Docker Enterprise Support
 
If you encounter any issue on a Windows node (host) in a UCP cluster, Docker Enterprise Support can assist.
 
Examples:
   - Unable to join Windows host as a worker node in a UCP cluster
   - Unable to pull a container image from DTR
   - LDAP synchronization with Active Directory fails on UCP cluster
 
 _If investigation reveals issue with Windows components, Docker may ask you to file a case with Microsoft._
 
## What's Next
 
[Comparison of Docker EE Standard and Advanced](https://www.docker.com/pricing)

