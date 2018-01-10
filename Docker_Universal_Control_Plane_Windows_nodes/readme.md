---
type: kbase
version: 5
title: "Docker Universal Control Plane Windows nodes"
summary: "Before adding a Windows node to your UCP cluster, you must set up the Docker daemon on that Windows node to listen to a specific port, and you must open up certain firewall settings. You can do this by running a Powershell script that is included with UCP. It is recommended that you run this script on every Windows node before installing UCP. If you'd just like to see what the script does before running it, you can run:"
dateCreated: "Mon, 24 Jul 2017 21:09:55 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 2
internal: no
author: "joaofnfernandes"
platform:
  - windows
testedon:
tags:
product:
  - EE
---

Before adding a Windows node to your UCP cluster, you must set up the Docker daemon on that Windows node to listen to a specific port, and you must open up certain firewall settings. You can do this by running a Powershell script that is included with UCP.

It is recommended that you run this script on every Windows node before installing UCP.

Run the script in a Powershell console:

    docker run --rm docker/ucp-agent-win:2.2.0 windows-script | powershell -noprofile -noninteractive -command 'Invoke-Expression -Command $input'

If you'd just like to see what the script does before running it, you can run:

    docker run --rm docker/ucp-agent-win:2.2.0 windows-script