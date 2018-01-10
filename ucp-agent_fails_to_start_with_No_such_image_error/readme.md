---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: ucp-agent fails to start with No such image error
internal: no             # set to yes to keep it internal-only
comment: ""
type: kbase               
author:  "squizzi"
product:       
  - ee    
testedon:
  - ucp-2.1.3
platform:           # Optional. Keep all that apply.
  - linux
  - windows server
  - z systems
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - error
  - daemon
---
## Issue

`ucp-agent` containers fail to start on some nodes with the error: `No such image` and the following error appears in Docker daemon logs:

```
Nov 10 21:36:34 example-2 dockerd[10682]: time="2017-11-10T21:36:34.288953150Z" level=error msg="pulling image failed" error="Get https://registry-1.docker.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)" module="node/agent/taskmanager" task.id=ifd77bjeha5337m6ghbtz1ecv
Nov 10 21:36:34 example-2 dockerd[10682]: time="2017-11-10T21:36:34.289652275Z" level=error msg="fatal task error" error="No such image: docker/ucp-agent@sha256:8dacacfe275db3cacea1ffdfb1474996f15ab6a4c698425a0bd3eec40313477b" module="node/agent/taskmanager" task.id=ifd77bjeha5337m6ghbtz1ecv
```

This error can also be seen for other images, not necessarily `ucp-agent`.

## Prerequisites

This issue occurs on environments where:

- `docker load` has been used to load UCP images onto nodes

## Root Cause

This issue is entirely caused by a limitation within the `docker load` command. It occurs on disconnected environments when all images have been loaded using `docker load`.  1 manager node is then connected to the Internet via `docker-proxy` or another method to facilitate image pulling, however other machines within the cluster are unable to connect to the Internet.

When an image is loaded via `docker load,` it lacks a `RepoDigest` which can be seen with the `docker images --digests` command.  If the image is pulled from a registry such as DTR or Docker Hub the image will contain a `RepoDigest` — note the two `ucp-agent` images below.  One was loaded with `docker load` while the other was pulled with the command `docker pull docker/ucp-agent:2.1.3`.  The images are identical, but the one loaded lacks a digest:

```
docker/ucp-agent        2.1.3               <^>sha256:8dacacfe275db3cacea1ffdfb1474996f15ab6a4c698425a0bd3eec40313477b<^^>   5cfdc43bb247        7 months ago        22.5 MB

...

docker/ucp-agent        2.1.3               <^><none><^^>              5cfdc43bb247        7 months ago        22.5 MB
```

When the connected manager node is elected as leader it routinely performs a `docker service update` of `ucp-agent`.  During this process, it pins the image to be used on other machines based on the Docker images that already remain within the manager's image cache.  When this occurs, the service update attempts to update to the digested version of `ucp-agent` on the other disconnected nodes which cannot `docker pull` that image.  Since the `docker pull` fails, the nodes rely on their local image cache which only contains the `docker load`ed image which as seen above contains no `RepoDigest`.  This results in the `No such image` error.

## Resolution

To resolve the issue with `ucp-agent` triggering `No such image` errors, perform the following steps:

1. Determine the UCP manager node which contains the digested version of the image by examining the output of `docker images -a --digests`. See the 'Root Cause' section below for more details.

2. Remove the problematic image from the node where the `ucp-agent` service originated from:

```
$ sudo docker rmi -f docker/ucp-agent:2.1.3
Untagged: docker/ucp-agent:2.1.3
Untagged: docker/ucp-agent@sha256:8dacacfe275db3cacea1ffdfb1474996f15ab6a4c698425a0bd3eec40313477b
```

3. Then reload the images to get the non-digested `ucp-agent`:

```
$ sudo docker load < ucp_images_2.1.3.tar.gz
```

4. Finally perform a `docker service update` to force the service to pin at the image loaded versus the digest image pulled:

```
$ sudo docker service update --image docker/ucp-agent:2.1.3 ucp-agent --force
```

5. This should result in the task running on the node entering a 'Running' state after being rejected several times due to not being able to find the `ucp-agent` image. Verify with the following:

```
$ sudo docker service ps ucp-agent
ID            NAME                                     IMAGE                   NODE                DESIRED STATE  CURRENT STATE            ERROR                             PORTS
bhd38r52sbuq  ucp-agent.mguqm4ngywdje8fy0ja5rmqkn      docker/ucp-agent:2.1.3  adp-disconnected-2  Running        Running 3 minutes ago                                      
2jlbfisusurn  ucp-agent.l9qcwt7klogkfla0uc91kkguo      docker/ucp-agent:2.1.3  adp-disconnected-0  Running        Running 3 minutes ago                                      
zhg7tzv8zher  ucp-agent.mguqm4ngywdje8fy0ja5rmqkn      docker/ucp-agent:2.1.3  adp-disconnected-2  Shutdown       Shutdown 3 minutes ago                                     
bu1frmwdjms7  ucp-agent.l9qcwt7klogkfla0uc91kkguo      docker/ucp-agent:2.1.3  adp-disconnected-0  Shutdown       Shutdown 4 minutes ago                                     
czpnfuo2b979  ucp-agent.mguqm4ngywdje8fy0ja5rmqkn      docker/ucp-agent:2.1.3  adp-disconnected-2  Shutdown       Rejected 5 minutes ago   "No such image: docker/ucp-age…"  
vzi2f2oktf48   \_ ucp-agent.mguqm4ngywdje8fy0ja5rmqkn  docker/ucp-agent:2.1.3  adp-disconnected-2  Shutdown       Rejected 5 minutes ago   "No such image: docker/ucp-age…"  
qzy6rscgo8bc   \_ ucp-agent.mguqm4ngywdje8fy0ja5rmqkn  docker/ucp-agent:2.1.3  adp-disconnected-2  Shutdown       Rejected 6 minutes ago   "No such image: docker/ucp-age…"  

$ sudo docker service ls
ID            NAME       MODE    REPLICAS  IMAGE
ifc259nrv60z  ucp-agent  global  2/2       docker/ucp-agent:2.1.3
```
