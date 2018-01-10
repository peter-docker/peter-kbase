---
type: kbase
version: 8
title: "Why do I get a warning when I have only two controllers/managers in my cluster?"
summary: "For Docker Universal Control Plane to be highly available and fault-tolerant, you need to join multiple controller nodes (also known as manager nodes): With only two controller nodes, UCP doesn't tolerate any failures. You should add additional controller nodes as soon as possible. You can also join as many worker nodes as you like, to be able to handle more workloads. To join more nodes to UCP, navigate to the 'Nodes' page and click 'Add Node'."
dateCreated: "Fri, 29 Apr 2016 18:59:47 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 2
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
| **2** | **0** |
| 3 | 1 |
| 5 | 2 |
| 7 | 3 |

With only two controller nodes, UCP doesn't tolerate any failures. You should add additional controller nodes as soon as possible.

For high availability you should have 3, 5, or 7 controllers nodes. More than 7 can negatively impact the performance of UCP. You can also join as many worker nodes as you like, to be able to handle more workloads. To join more nodes to UCP, navigate to the 'Nodes' page and click 'Add Node'.

> **Note:** To learn more about UCP High Availability, visit <http://docs.docker.com/ucp/high-availability/set-up-high-availability/>.
