---
title: "Can I create objects and deploy services from a local Docker client?"
internal: no
comment:
type: kbase
author: "lmuchnick"
product:
  - ee
testedon:
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.4
platform:
  - linux
  - windows server
  - z systems
tags:
  - access control
  - daemon
  - installing
  - networking
  - security
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
uniqueid: KB000197
---

TESTING TESTING TESTING AND MORE TESTING!!!

A client certificate bundle allows end users to create objects and deploy services from a local Docker client rather than having to ssh directly into a manager node.

The Docker CLI client includes the client certificates as part of the request to Docker Engine. The Docker CLI can then be used to create services, networks, volumes, and other resources on a swarm managed by UCP.

To create a client bundle, log into UCP, and click your login name in the upper left. Select **My Profile** -> **Client Bundles**. Then click **New Client Bundle**.

After downloading the bundle, see the [UCP documentation](https://docs.docker.com/datacenter/ucp/2.2/guides/user/access-ucp/cli-based-access/#use-client-certificates) for installation instructions.

To verify that a client certificate bundle has been loaded and that the client is successfully communicating with UCP, look for `ucp` in the `Server Version` returned by 'docker version'.

```
$ docker version --format '{{.Server.Version}}'

ucp/2.2.4
```

