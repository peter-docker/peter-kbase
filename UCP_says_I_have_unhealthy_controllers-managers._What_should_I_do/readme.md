---
type: kbase
version: 7
title: "UCP says I have unhealthy controllers/managers. What should I do?"
summary: "For Docker Universal Control Plane to be highly available and fault-tolerant, you need to join multiple controller nodes (also known as manager nodes): Controllers/managers Failures tolerated UCP displays a warning message when one or more controller nodes are not healthy, as this can reduce the fault-tolerance of UCP. Troubleshoot and find what's keeping these nodes unhealthy. To learn more about troubleshooting UCP, visit http://docs.docker.com/ucp/monitor/troubleshoot-ucp/."
dateCreated: "Fri, 29 Apr 2016 19:01:06 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 6
internal: no
author: "justinnevill"
platform:
testedon:
tags:
product:
  - EE
---

For Docker Universal Control Plane to be highly available and fault-tolerant, you need to join multiple controller nodes (also known as manager nodes):

| Controllers/managers | Failures tolerated |
| --- | --- |
| 1 | 0 |
| 3 | 1 |
| 5 | 2 |
| 7 | 3 |

UCP displays a warning message when one or more controller nodes are not healthy, as this can reduce the fault-tolerance of UCP. Troubleshoot and find what's keeping these nodes unhealthy.

> To learn more about troubleshooting UCP, visit <http://docs.docker.com/ucp/monitor/troubleshoot-ucp/>.
