---
type: kbase
version: 6
title: "Application unreachable due to NodeJS tasks"
summary: "Load balancer unable to reach the application containers. Logs of both the load balancer and the application containers do not provide insight into any issue. Are there processes running around the same times as the outage(s) or backup scripts? Are the web application containers running? If none of the above solve the issue, check for unresponsiveness caused by NodeJS tasks. If NodeJS tasks are suspected, moved them to a different machine."
dateCreated: "Thu, 26 Jan 2017 15:24:09 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 0
internal: no
author: "hannahagee"
platform:
testedon:
tags:
product:
  - EE
uniqueid: KB000187
---

TESTING TESTING TESTING AND MORE TESTING!!!

## Summary

* Website becomes unreachable for a short period of time.
* Load balancer unable to reach the application containers.
* Logs of both the load balancer and the application containers do not provide insight into any issue.

## Troubleshooting

* Are there processes running around the same times as the outage(s) or backup scripts?
* Can you reach the containers by IP address and DNS record?
* Are the web application containers running?
* Is there any DNS outage on Docker Cloud (status.docker.com)?

## Resolution

If none of the above solve the issue, check for unresponsiveness caused by NodeJS tasks. If NodeJS tasks are suspected, moved them to a different machine.