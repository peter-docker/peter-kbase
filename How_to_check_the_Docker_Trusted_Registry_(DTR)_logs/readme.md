---
type: kbase
version: 2
title: "How to check the Docker Trusted Registry (DTR) logs"
summary: "You can check the Docker Trusted Registry (DTR) logs from a DTR node. From a DTR node, issue the command docker logs <container-id> for the DTR containers associated with these images: docker/dtr-nginx docker/dtr-api docker/dtr-registry docker/dtr-rethink docker/dtr-etcd"
upvote: 0
internal: no
author:
  - tfoxnc
  - hannahagee
platform:
testedon:
  - dtr-2.3.x
tags:
  - logging
product:
  - EE
---

You can check the Docker Trusted Registry (DTR) logs from a DTR node.

Retrieve the CONTAINER ID:

```
* docker/dtr-nginx
$ docker ps | grep docker/dtr-nginx

* docker/dtr-api
$ docker ps | grep docker/dtr-api

* docker/dtr-registry
$ docker ps | grep docker/dtr-registry

* docker/dtr-rethink
$ docker ps | grep docker/dtr-rethink

* docker/dtr-etcd
$ docker ps | grep docker/dtr-etcd

* docker/dtr-postgres
$ docker ps | grep docker/dtr-postgres

* docker/dtr-notary-signer
$ docker ps | grep docker/dtr-notary-signer

* docker/dtr-jobrunner
$ docker ps | grep docker/dtr-jobrunner

* docker/dtr-notary-server
$ docker ps | grep docker/dtr-notary-server

* docker/dtr-garant
$ docker ps | grep docker/dtr-garant
```

From your DTR node, issue the command `docker logs <^><container-id><^^>` for the DTR containers associated with these images while redirecting the output to a file:

```
$ docker logs <^><container-id><^^> > dtr-nginx.log
$ docker logs <^><container-id><^^> > dtr-api.log
$ docker logs <^><container-id><^^> > dtr-registry.log
$ docker logs <^><container-id><^^> > dtr-rethink.log
$ docker logs <^><container-id><^^> > dtr-etcd.log
$ docker logs <^><container-id><^^> > dtr-postgres.log
$ docker logs <^><container-id><^^> > dtr-notary-signer.log
$ docker logs <^><container-id><^^> > dtr-jobrunner.log
$ docker logs <^><container-id><^^> > dtr-notary-server.log
$ docker logs <^><container-id><^^> > dtr-garant.log
```

Finally gather the logs into a tar file:

```
$ tar -czf dtr.tar.gz dtr-*
```
