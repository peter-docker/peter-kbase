---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Manual install of CloudStor plugin fails with error
internal: no
comment: "internal comments that will only show up in the GH source file"
type: kbase
author:  quaddo
product: 
  - EE
platform: 
testedon: 
  - ee-17.06.1-ee-2
tags: 
  - Docker Cloud
  - Docker for Azure
  - Docker for AWS
  - error
---
## Issue

While trying to install the CloudStor plugin (eg, in Azure) it fails with this error:

`cloudstor.sock: connect: no such file or directory`.

For example:
```
[root@ucpleader ~]# docker plugin install --alias cloudstor:azure --grant-all-permissions docker4x/cloudstor:17.06.1-ee-2-azure1
17.06.1-ee-2-azure1: Pulling from docker4x/cloudstor
b1db7b698a5b: Download complete
Digest: sha256:3b7876a11c25243ab80c8b423d6cea2294d9466e677b5a38f589fc92baa69acc
Status: Downloaded newer image for docker4x/cloudstor:17.06.1-ee-2-azure1
Error response from daemon: dial unix /run/docker/plugins/18551fa88d2bb283e6a27c1c8077b415a8ca2dd1c6faf0da8d42672068e409ca/cloudstor.sock: connect: no such file or directory
```

Here we see that it is verified as not enabled:

```
[root@ucpleader ~]# docker plugin ls
ID                  NAME                DESCRIPTION                       ENABLED
18551fa88d2b        cloudstor:azure     cloud storage plugin for Docker   false
```

## Resolution

The manual installation of CloudStor is not a customer-supported activity. CloudStor is pre-installed and pre-configured in Docker swarms deployed through [Docker for AWS](https://www.docker.com/docker-aws) or [Docker for Azure](https://www.docker.com/docker-azure).

It's also worth noting that if you are trying to install any component (container, plugin) which begins with `docker4x/*` (eg, `docker4x/cloudstor`), doing so is not a customer-supported activity.

The supported path forward is to use one of the templated Docker cloud offerings:

1. [Docker for Azure](https://www.docker.com/docker-azure)
2. [Docker for AWS](https://www.docker.com/docker-aws)

## See Also

- [Docker for AWS persistent data volumes](https://docs.docker.com/docker-for-aws/persistent-data-volumes/)
- [Docker for Azure persistent data volumes](https://docs.docker.com/docker-for-azure/persistent-data-volumes/)

## Additional Questions

If you are interested in one of the templated solutions for Docker for Azure or Docker for AWS, [please start here](https://www.docker.com/pricing) or contact your account manager.
