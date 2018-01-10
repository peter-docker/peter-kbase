---
type: guide
version: 3
title: "Docker Universal Control Plane Windows Early Access"
summary: "This article outlines the steps required to set up a Docker UCP installation with Early Access support for Windows Server 2016 worker nodes. Please work with your account representative to join the Windows Early Access program and provide additional details on how to install UCP to enable Windows support. The Early Access Windows support requires manual steps to complete the join process for worker nodes."
dateCreated: "Tue, 10 Jan 2017 20:11:09 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "justinnevill"
platform:
  - windows server
testedon:
tags:
product:
  - EE
---

This article outlines the steps required to set up a Docker UCP installation with Early Access support for Windows Server 2016 worker nodes.

Please work with your account representative to join the Windows Early Access program and provide additional details on how to install UCP to enable Windows support.

## Manual Steps

The Early Access Windows support requires manual steps to complete the `join` process for worker nodes. After performing the initial join operation, the node will be reported in UCP as:

    [Pending] You must now reconfigure your windows worker node following instructions at https://www.docker.com/ddc-win-ea

From a PowerShell terminal running as `Administrator`, run the following commands:

    netsh advfirewall firewall add rule name="docker_ucp" dir=in action=allow protocol=TCP localport=12376
    Stop-Service docker
    dockerd --unregister-service
    dockerd -H npipe:// -H 0.0.0.0:12376 --tlsverify --tlscacert=c:\ProgramData\docker\ucp\ca.pem --tlscert=c:\ProgramData\docker\ucp\cert.pem --tlskey=c:\ProgramData\docker\ucp\key.pem --register-service
    Start-Service docker

## Limitations

* The UCP Windows Server Early Access support is limited to worker nodes only. You can not run a Windows Server node as a manager.
* If you have not enabled Windows support during install, Windows worker nodes can not be joined to the cluster later.
