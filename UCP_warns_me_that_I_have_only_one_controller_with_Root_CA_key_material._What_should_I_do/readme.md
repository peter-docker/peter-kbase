---
type: kbase
version: 5
title: "UCP warns me that I have only one controller with Root CA key material. What should I do?"
summary: "If all the Controller(s) containing this Root CA material are offline, users can not download new certificate bundles, and new nodes can not be joined to the cluster until at least one Controller with the root material is available. We recommend you replicate your Root CA material to at least one additional controller so you will not have a single-point-of-failure if your initially installed controller goes offline."
dateCreated: "Fri, 29 Apr 2016 19:01:25 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 5
internal: no
author: "justinnevill"
platform:
testedon:
  - ucp-1.1.1
tags:
  - certs
product:
  - EE
---

When Docker Universal Control Plane 1.1 is deployed in a Highly Available configuration, you may choose to replicate the Root CA certificates and private keys to zero or more controllers. During the initial Controller installation, this Root CA material is generated. If all the Controller(s) containing this Root CA material are offline, users can not download new certificate bundles, and new nodes can not be joined to the cluster until at least one Controller with the root material is available. Existing nodes and user bundles will continue to work.

We recommend you replicate your Root CA material to at least one additional controller so you will not have a single-point-of-failure if your initially installed controller goes offline. This replication process can be accomplished by running the `backup --root-ca-only` command in the `docker/ucp` tool on the initial controller, and then run `restore --root-ca-only` on one more more replica controllers.

> **Note:** To learn more about the Certificate Authority features of UCP, visit <http://docs.docker.com/ucp/high-availability/replicate-cas/>.
