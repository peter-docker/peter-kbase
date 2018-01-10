---
type: kbase
version: 4
title: "How can I use Bash on Ubuntu on Windows to push images to Docker Hub?"
summary: ""
dateCreated: "Tue, 30 Aug 2016 23:59:00 GMT"
dateModified: "Tue, 30 Aug 2016 23:59:00 GMT"
dateModified_unix: 1472601540
upvote: 2
internal: no
author: "yiming"
platform:
  - windows
testedon:
tags:
product:
  - Hub
---

This article describes how to use Bash on Ubuntu on Windows to push/pull images to Docker Hub.

## Prerequisites 

* 64-bit version of Windows 10 Pro, Enterprise, or Education, Anniversary Update build 14316 or later
* Microsoft Hyper-V

## Steps

1. Download and install [Docker for Windows](https://docs.docker.com/docker-for-windows/). 
1. Ensure that Docker for Windows successfully starts.
1. Set up Bash on Ubuntu on Windows, as per the instructions on the MSDN website. This will function as your Docker server.
1. Run the following commands on Bash on Ubuntu on Windows to set up your Docker client on Bash:
    ```
    $ curl -sSL https://get.docker.com/ | sh
    $ sudo usermod -aG docker <^><your_bash_username><^^>
    ```
1. Close the existing Bash window, and open a new one.
1. Add the `-H tcp://0.0.0.0:2375` parameter to all your Docker commands. For example:
    ```
    docker -H tcp://0.0.0.0:2375 login
    docker -H tcp://0.0.0.0:2375 pull alpine
    docker -H tcp://0.0.0.0:2375 tag alpine <username>/<repo>
    docker -H tcp://0.0.0.0:2375 push <username>/<repo>
    ```
