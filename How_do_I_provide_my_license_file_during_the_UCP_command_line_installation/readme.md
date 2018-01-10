---
type: kbase
version: 3
title: "How do I provide my license file during the UCP command line installation?"
summary: "Go to Docker Store and download your UCP license if you don't already have the file. When installing UCP, use the -v option to create a volume for the license file (shown in red): docker run --rm -it \ --name ucp \ -v /var/run/docker.sock:/var/run/docker.sock \ -v /path/to/my/docker_subscription.lic:/docker_subscription.lic \ docker/ucp \ install [command options]"
dateCreated: "Thu, 28 Jul 2016 18:22:08 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "sabin.basyal"
platform:
testedon:
tags:
  - installing
product:
  - EE
---

Go to [Docker Store](https://store.docker.com/bundles/docker-datacenter) and download your UCP license if you don't already have the file.

When installing UCP, use the `-v` option to create a volume for the license file (shown in red):

    docker run --rm -it \
      --name ucp \
      -v /var/run/docker.sock:/var/run/docker.sock \      
      -v /path/to/my/docker_subscription.lic:/docker_subscription.lic \
      docker/ucp \
      install [command options]
