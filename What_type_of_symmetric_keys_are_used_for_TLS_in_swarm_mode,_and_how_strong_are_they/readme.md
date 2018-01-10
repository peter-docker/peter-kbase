---
type: kbase
version: 6
title: "What type of symmetric keys are used for TLS in swarm mode, and how strong are they?"
summary: "The keys used for communication over TLS in swarm mode are Elliptic Curve (ECDSA) keys created with a length of 256 bits. You can check the strength of your swarm keys yourself by looking at the files located on a Swarm manager in /var/lib/docker/swarm/certificates/* and viewing the details of your Swarm certificates with the openssl command. This will tell you about the certificates used for swarm mode, as well as the encryption type and key strength used for Swarm mode."
dateCreated: "Wed, 08 Mar 2017 21:16:36 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "curtisz"
platform:
testedon:
tags:
product:
  - EE
---

The keys used for communication over TLS in swarm mode are Elliptic Curve (ECDSA) keys created with a length of 256 bits. The strength of an Elliptic Curve key of 256 bits is roughly equivalent to that of a 3072-bit RSA key. You can check the strength of your swarm keys yourself by looking at the files located on a Swarm manager in `/var/lib/docker/swarm/certificates/*` and viewing the details of your Swarm certificates with the `openssl` command. For example:

    sudo openssl x509 -text -in /var/lib/docker/swarm/certificates/swarm-node.crt

This will tell you about the certificates used for swarm mode, as well as the encryption type and key strength used for Swarm mode.

Additionally, you can take a look [at a section of the Swarmkit source within the Docker project where this is explicitly set](https://github.com/docker/swarmkit/blob/01329d9725b66e01c130ec6fecdde7c93f0e994a/ca/certificates.go#L38-L50 "https://github.com/docker/swarmkit/blob/01329d9725b66e01c130ec6fecdde7c93f0e994a/ca/certificates.go#L38-L50").