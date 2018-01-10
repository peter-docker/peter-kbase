---
type: kbase
version: 5
title: "With Content Trust enabled, why do I get a 'no such host' error for 'notary.docker.io'?"
summary: "If you want to use the Notary service available in your DTR cluster, you do not want Docker to poll the default 'notary.docker.io'. In this case, you will either need to set this environment variable to your own DTR address, or leave this value empty to have Docker infer the location of your Notary service from the repository in the image name."
dateCreated: "Wed, 22 Feb 2017 21:24:48 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "curtisz"
platform:
testedon:
tags:
  - Docker Content Trust
  - error
product:
  - EE
---

With Docker Content Trust capability enabled, you may see the following error:

    "Error creating container: Get https://notary.docker.io: dial tcp: lookup notary.docker.io <^><your_DNS><^^>: no such host"

With Docker Content Trust enabled, your cluster will only run images which have been signed by the image creator. Docker attempts to use the value of the `DOCKER_CONTENT_TRUST_SERVER` environment variable to determine which Notary server to use for image signing metadata like signatures. If you want to use the Notary service available in your DTR cluster, you do not want Docker to poll the default "notary.docker.io". In this case, you will either need to set this environment variable to your own DTR address, or leave this value empty to have Docker infer the location of your Notary service from the repository in the image name.
