---
type: kbase
version: 2
title: "Installing UCP 1.0 to replace a beta version"
summary: "In order to take advantage of UCP 1.0, it will be necessary for you to first fully uninstall the UCP Beta. To ensure a smooth transition process, it is very important that you use the latest UCP 1.0 image for both the uninstall and install commands. If you haven’t already, please be sure to install the latest release of CS Engine 1.10 on any production nodes to be used with UCP 1.0."
dateCreated: "Tue, 15 Mar 2016 18:34:13 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "nhazlett"
platform:
  - linux
testedon:
  - ucp-1.0.0
tags:
  - installing
product:
  - EE
---

## Overview

In order to take advantage of UCP 1.0, it will be necessary for you to first fully uninstall the UCP Beta.

## Goal

In order to take advantage of UCP 1.0, it will be necessary for you to first fully uninstall the UCP Beta. In addition, UCP 1.0 will require a fresh install as it will not be possible to upgrade from previous beta versions (0.9 or below) to 1.0. To ensure a smooth transition process, it is very important that you use the latest UCP 1.0 image for both the uninstall and install commands.

## Steps

### First Step

Begin byuninstalling the UCP Beta from all of your nodes. It is best to uninstall regular nodes first, and then uninstall controller nodes. Be sure to use the latest 1.0.0 UCP image for uninstall, which might look something like this:

`docker run --rm -it --name ucp -v /var/run/docker.sock:/var/run/docker.sock docker/ucp:1.0.0 uninstall -i`

### Second Step

If you haven’t already, please be sure to install the latest release of CS Engine 1.10 on any production nodes to be used with UCP 1.0. You can find instructions for installing CS Engine 1.10 here: [https://docs.docker.com/ucp/producti...e-on-each-node](https://docs.docker.com/ucp/production-install/#step-3-install-docker-cs-engine-on-each-node "https://docs.docker.com/ucp/production-install/#step-3-install-docker-cs-engine-on-each-node")

Next, do a fresh install of UCP using the latest 1.0.0 image. It is recommended that you follow the newly released UCP 1.0 documentation, but an example might look like:

`docker run --rm -it --name ucp -v /var/run/docker.sock:/var/run/docker.sock docker/ucp install -i`

## What's Next

You should now have a fresh install of UCP 1.0. You can find a full overview and documentation of UCP here: [https://docs.docker.com/ucp/overview/](https://docs.docker.com/ucp/overview/ "https://docs.docker.com/ucp/overview/")
*Tags recommended by the template: *[article:howto](#)
1.
