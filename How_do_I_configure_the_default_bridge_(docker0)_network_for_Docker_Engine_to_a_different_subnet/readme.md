---
type: kbase
version: 5
title: "How do I configure the default bridge (docker0) network for Docker Engine to a different subnet?"
summary: "How do I configure the default bridge (docker0) network for Docker Engine to a different subnet? You can configure the default bridge network by providing the bip option along with the desired subnet in the daemon.json (default location at /etc/docker/daemon.json on Linux) file as follows: For further information on configuring the default bridge network and the options available, refer to https://docs.docker.com/engine/userguide/networking/default_network/custom-docker0/."
dateCreated: "Mon, 15 Aug 2016 07:46:37 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 23
internal: no
author: "kenny.lim"
platform:
testedon:
tags:
  - networking
product:
  - EE
---

The Docker Engine default bridge network is conflicting with our internal network hosts access. How do I configure the default bridge (`docker0`) network for Docker Engine to a different subnet?

You can configure the default bridge network by providing the `bip` option along with the desired subnet in the `daemon.json` (default location at `/etc/docker/daemon.json` on Linux) file as follows:

    {
      "bip": "<^>172.26.0.1/16<^^>"
    }

Then restart the docker daemon (`sudo systemctl restart docker` on systemd based Linux operating systems).

For further information on configuring the default bridge network and the options available, refer to [https://docs.docker.com/engine/userguide/networking/default_network/custom-docker0/](https://docs.docker.com/engine/userguide/networking/default_network/custom-docker0/ "https://docs.docker.com/engine/userguide/networking/default_network/custom-docker0/").
