---
type: kbase
version: 2
title: "How do you set custom docker daemon options in the Docker Cloud Agent?"
summary: "How do you set custom docker daemon options in the Docker Cloud Agent? Options can be passed to the Docker daemon that the Docker Cloud Agent runs by: The options currently in effect can be confirmed after restarting the Cloud Agent by runningps faux | grep dockeron the command line and reviewing the options shown for the Docker daemon process."
dateCreated: "Thu, 18 Feb 2016 17:22:59 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 1
internal: no
author: "programmerq"
platform:
  - linux
testedon:
tags:
product:
  - Hub
---

## Question

* How do you set custom docker daemon options in the Docker Cloud Agent?
* Can I customize the docker engine options used by the Docker Cloud Agent?

## Answer

Options can be passed to the Docker daemon that the Docker Cloud Agent runs by:

1. Stop the agent
2. Run `dockercloud-agent -docker-opts <extra opts>` where `<extra opts>` are the options you would like to be passed to the Docker daemon
3. Restart the agent

These extra options will get stored in the `/etc/dockercloud/agent/dockercloud-agent.conf` file and will be in effect for all future runs of the Docker Cloud Agent. The options currently in effect can be confirmed after restarting the Cloud Agent by running`ps``faux | grep``docker`on the command line and reviewing the options shown for the Docker daemon process.