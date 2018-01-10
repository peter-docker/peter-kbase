---
type: kbase
version: 3
title: "The Docker port is externally accessible on this node"
summary: "The Docker port is externally accessible on this node, accepting connections on port <port>. This node is insecure. This means that you've configured Docker Engine running on the node to listen to connections on TCP port 2375, and potentially anyone can use these ports to run Docker commands in your node. Since these commands bypass UCP and are sent directly to Docker Engine, UCP won't be able to enforce any role-based access control policy on those requests."
dateCreated: "Fri, 20 Jan 2017 19:57:52 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "justinnevill"
platform:
testedon:
tags:
product:
  - EE
---

UCP secures your nodes with role-based access control, but for that to happen all requests must be handled by UCP. If you're seeing the error:

    The Docker port is externally accessible on this node, accepting connections on port <^><port><^^>. This node is insecure.

This means that you've configured Docker Engine running on the node to listen to connections on TCP port 2375, and potentially anyone can use these ports to run Docker commands in your node.

![](./images/image.png)

Since these commands bypass UCP and are sent directly to Docker Engine, UCP won't be able to enforce any role-based access control policy on those requests.

You should either configure Docker Engine to not listen to TCP requests, or [use mutual TLS](https://docs.docker.com/engine/security/https/).

If you're seeing the warning:

    Unauthorized users may be able to access this node since it's listening on port <^><port><^^>

This means that you've configured Docker Engine to listen to connections on port 2376. While the communications between clients and Docker Engine are encrypted, this can still allow unauthorized users to run Docker commands.

Make sure you're using mutual TLS, or even better, configure Docker Engine to not listen to TCP requests.
