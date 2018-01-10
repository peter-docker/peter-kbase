---
type: kbase
version: 3
title: "How do I set the Docker daemon options?"
summary: "All the Docker daemon options are documented at https://docs.docker.com/engine/refer...ndline/daemon/. The exact process to set these options varies by the way you launch the Docker daemon: Once you have set the options for the daemon, restart the Docker daemon. You can confirm that your changes have taken place by executing the ps faux | grep docker command, looking for the daemon, and confirming that your options are there."
dateCreated: "Thu, 18 Feb 2016 20:46:31 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "programmerq"
platform:
testedon:
tags:
  - daemon
product:
  - EE
---

All the Docker daemon options are documented at [https://docs.docker.com/engine/refer...ndline/daemon/](https://docs.docker.com/engine/reference/commandline/daemon/ "https://docs.docker.com/engine/reference/commandline/daemon/").

The exact process to set these options varies by the way you launch the Docker daemon:

* boot2docker/docker-machine — use `EXTRA_ARGS` in `/var/lib/boot2docker/profile` - [https://github.com/boot2docker/boot2...tent-partition](https://github.com/boot2docker/boot2docker/blob/master/doc/FAQ.md#local-customisation-with-persistent-partition "https://github.com/boot2docker/boot2docker/blob/master/doc/FAQ.md#local-customisation-with-persistent-partition")

* systemd (Ubuntu, Debian, RHEL 7, CentOS 7, Fedora, Archlinux)— `systemctl edit docker.service`, change the `ExecStart` line

* `/etc/default/docker` (Ubuntu 12.04) —`DOCKER_OPTS` in `/etc/default/docker`

Once you have set the options for the daemon, restart the Docker daemon. You can confirm that your changes have taken place by executing the `ps faux | grep docker` command, looking for the daemon, and confirming that your options are there.
