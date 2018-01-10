---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: What are known parities between Windows Server, Linux, and IBM Z Systems?
internal: no       # if not yes, then optional
comment: "INTERNAL version available at https://docker.atlassian.net/wiki/spaces/PM/pages/137643096/Docker+EE+Infrastructure+Feature+Parity"
type: kbase
author: "eiichik"
dateModified: "Wed, 11 Oct 2017 14:52:30 GMT"
dateModified_unix: 1507751550
product: EE
platform:
  - windows server
  - linux
  - z systems
testedon: 
  - ee-17.06.2-ee-3
  - ucp-2.2.0
---

Docker Engine includes common features across different infrastructures for the most part. However, there are few parities.

The assumption is that all features work on x86 Linux unless otherwise stated.

The following table shows known parities for Windows Server and IBM Z Systems as of Docker EE 17.06.2-ee-3:
 
| ***Feature Set*** | ***Windows Server 2016*** | ***IBM Z Systems (s390x) Linux*** |
| ----------------- | :----------------------------- | ------------------------------- |
| EE Architecture and Installation | <ul><li> No UCP managers and DTR replicas (UCP workers only)</li> <li><b>ucp-agent-win</b> is used to manage nodes.</li> <li> Few additional steps are required as described on documentation. </li> <li> No usage statistics available for disk usage yet </li></ul>| <ul><li>No UCP managers and DTR replicas (UCP workers only</li><li><b>ucp-agent-s390x</b> is used to manage nodes </li></ul> | 
| Orchestration | [Known issue with "--global" option](https://success.docker.com/KBase/'--mode_global'_can_cause_platform_mismatch_for_replica) | |
| Networking | <ul><li> IP Routing Mesh is not available available yet </li> <li>Encrypted overlay networks is not supported </li></ul>| |
| Security | <ul><li>v2 Plug-in is not supported yet </li></ul> | |
| Plug-Ins | <ul><li>v1 Plug-ins is supported if provider compiled it to work on Windows platform</li> <li>Encrypted overlay networks is not supported </li></ul> | |
| Volume | <ul><li>Only empty directory can be attached as volume</ul></li> | |




