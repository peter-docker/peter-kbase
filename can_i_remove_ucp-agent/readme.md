---
title: "Can I remove the ucp-agent service(s)?"
internal: no
comment:
type: kbase
author: "bryceryan-docker"
product:
  - ee
testedon:
platform:
  - linux
  - windows server
  - z systems
tags:
  - uninstalling
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
uniqueid: KB000208
---
TESTING TESTING TESTING AND MORE TESTING!!!

Can I remove the ucp-agent service from my cluster?

No, you must not remove the ucp-agent service from any node on your cluster, nor from the service list, even if the
service is not running. The ucp-agent service performs system tasks on behalf of the UCP manager. If you remove the service,
your cluster will not operate correctly.

If you do inadvertantly remove the service, please contact Docker Technical Support, as well as review
the operational practices leading to its removal. 
