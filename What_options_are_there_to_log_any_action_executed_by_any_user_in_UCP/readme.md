---
type: kbase
version: 3
title: "What options are there to log any action executed by any user in UCP"
summary: "Actions on a container such as create/start/stop/rm aren't logged by UCP, and UCP container actions are not logged with any of the default log drivers. To log container actions such as start/stop/remove, install a more comprehensive logging utility. This application has the ability to log run/stop/remove and other container behavior. Sysdig's Falco is open source and requires the installation of a kernel module on each node to be monitored."
dateCreated: "Thu, 15 Dec 2016 14:55:20 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
testedon:
  - ucp-2.0.0
tags:
product:
  - EE
---

Actions on a container such as create/start/stop/rm aren't logged by UCP, and UCP container actions are not logged with any of the default log drivers.

To log container actions such as start/stop/remove, install a more comprehensive logging utility. Many are available, but one such option is [Sysdig's Falco](http://www.sysdig.org/falco/ "http://www.sysdig.org/falco/"), an activity monitor with support for Docker containers. This application has the ability to log run/stop/remove and other container behavior. It also has some powerful behavior detection capabilities as well as network logging capabilities. Sysdig's Falco is open source and requires the installation of a kernel module on each node to be monitored.