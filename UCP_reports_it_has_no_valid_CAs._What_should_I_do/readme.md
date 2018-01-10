---
type: kbase
version: 7
title: "UCP reports it has no valid CAs. What should I do?"
summary: "If all the Controller(s) containing this Root CA material are offline, users can not download new certificate bundles, and new nodes can not be joined to the cluster until at least one Controller with the root material is available. If you are unable to recover this controller, you may use the docker/ucp tools regen-certs command with --root-ca-only , however, after regenerating the Root CA material, all users will have to download new bundles, and all nodes must be re-joined to the cluster."
dateCreated: "Fri, 29 Apr 2016 19:01:16 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "justinnevill"
platform:
testedon:
tags:
  - certs
product:
  - EE
---

When Docker Universal Control Plane is deployed in a Highly Available configuration, you may choose to replicate the Root CA certificates and private keys to zero or more controllers. During the initial Controller installation, this Root CA material is generated. If all the Controller(s) containing this Root CA material are offline, users can not download new certificate bundles, and new nodes can not be joined to the cluster until at least one Controller with the root material is available. Existing nodes and user bundles will continue to work.

To recover you should revive the initial controller, or replica with Root CA material.

If you are unable to recover this controller, you may use the `docker/ucp` tools `regen-certs` command with `--root-ca-only` , however, after regenerating the Root CA material, all users will have to download new bundles, and all nodes must be re-joined to the cluster.

> To learn more about the Certificate Authority features of UCP, visit <http://docs.docker.com/ucp/high-availability/replicate-cas/>
