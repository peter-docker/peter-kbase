---
type: kbase
version: 2
title: "Docker fails to push to DTR and requests authentication"
dateCreated: "Thu, 17 Mar 2016 19:46:40 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 1
internal: no
author: "kizbitz"
platform:
testedon:
tags:
    - error
product:
  - EE
---

# Overview

The `docker push` command fails and requests authentication when pushing to DTR.

# Symptoms

    docker login <^>dtr.example.com<^^>
    Username: <user>
    Password: <password>
    Email:
    WARNING: login credentials saved in /home/<^>username<^^>/.docker/config.json
    Login Succeeded
    <user>@docker ~ $ docker tag hello-world dtr.example.com/hello-world
    <user>@docker ~ $ docker push dtr.example.com/hello-world
    The push refers to a repository [dtr.example.com/hello-world]
    5f70bf18a086: Preparing
    b652ec3a27e7: Preparing    
    <^>unauthorized: authentication required<^^>
    <user>@docker ~ $ docker tag hello-world dtr.example.com/dev-test/hello-world
    <user>@docker ~ $ docker push dtr.example.com/dev-test/hello-world
    The push refers to a repository [dtr.example.com/dev-test/hello-world]
    5f70bf18a086: Preparing
    b652ec3a27e7: Preparing
    <^>unauthorized: authentication required<^^>
    <user>@docker ~ $ docker push dtr.example.com/<username>/dev-test/hello-world
    The push refers to a repository [dtr.example.com/<username>/dev-test/hello-world]
    5f70bf18a086: Preparing
    b652ec3a27e7: Preparing
    <^>unauthorized: authentication required<^^>

# Resolution

When pushing to a DTR server there are two things to keep in mind:

* DTR requires a namespace.
* The repository must be created in the UI (and have the proper permissions set) before a push is allowed.
