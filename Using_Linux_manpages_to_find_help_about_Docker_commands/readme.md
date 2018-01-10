---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Using Linux manpages to find help about Docker commands
internal: no
comment: "This should be extremely useful for learning customer facing and internal"
type: kbase
author:  aathomas
product: 
  - EE
platform:
  - linux
testedon:
tags:
---
## Issue

At times, finding point release specific instructions can seem overwelming, but this information is available in the man pages.

`man` is an interface to the online reference manuals. man is the system's manual pager. Each page argument given to man is normally the name of a program, utility, or function. 

More information can be found with the command `man man` on any Linux command line.

## Resolution

For example, `docker create network --help` doesn't provide enough information to learn how to create an encrypted overlay network.

To use `man` to find the information, add a hyphen between the commands to view the specific manpages for a specific Docker release outlined such as:
```
$ man docker-network-create
```

Manpages can processed using pipes and grep for ease of use to get to information quickly. For example:
```
$ man docker-network-create|grep -i encrypt -B5
```
From this information, you can determine that the following command can be used to setup an encrypted overlay network.

```
$ docker network create -d overlay \
       --subnet=10.11.0.0/16 \
       --ingress \
       --opt com.docker.network.mtu=9216 \
       --opt encrypted=true \                
```

