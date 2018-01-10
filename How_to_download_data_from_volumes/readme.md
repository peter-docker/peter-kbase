---
type: kbase
version: 3
title: "How to download data from volumes"
summary: "How to download data from volumes? If you want to backup your data volumes that are attached to running containers in Docker Cloud, you can perform the following steps: Run a SSH service that mounts the volumes of the service you want to backup (replacing mysql with the actual service name): Run scp to download the files of the data volume (replacingdownloader-1.uuid.cont.dockerapp.io with your container FQDN and/var/lib/mysql with the path you want to download):"
dateCreated: "Tue, 06 Oct 2015 16:59:45 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "justinnevill"
platform:
testedon:
tags:
  - Docker Cloud
  - backup
product:
  - Hub
---

## Question

How you I download data from volumes?

## Answer

If you want to backup your data volumes that are attached to running containers in Docker Cloud, you can perform the following steps:

### Downloading volume data locally using the CLI

Run a SSH service that mounts the volumes of the service you want to backup (replacing `mysql` with the actual service name):

    $ docker-cloud service run -n downloader -p 22:2222 -e AUTHORIZED_KEYS="$(cat ~/.ssh/id_rsa.pub)" --volumes-from mysql ubuntu

Run `scp` to download the files of the data volume (replacing`downloader-1.uuid.cont.dockerapp.io` with your container FQDN and`/var/lib/mysql` with the path you want to download):

    scp -r -P 2222 root@downloader-1.uuid.cont.dockerapp.io:/var/lib/mysql .
