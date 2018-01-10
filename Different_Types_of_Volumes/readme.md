---
type: guide
version: 5
title: "Different Types of Volumes"
summary: "A host volume lives on the Docker host's filesystem and can be accessed from within the container. An anonymous volume is useful for when you would rather have Docker handle where the files are stored. It can be difficult, however, to refer to the same volume over time when it is an anonymous volumes. A named volume is similar to an anonymous volume. Docker manages where on disk the volume is created, but you give it a volume name."
dateCreated: "Tue, 26 Apr 2016 18:50:37 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "programmerq"
platform:
testedon:
tags:
product:
  - EE
---

There are three types of volumes: *host*, *anonymous*, and *named:*

* A **host volume** lives on the Docker host's filesystem and can be accessed from within the container. To create a host volume: 
    
        docker run -v <^>/path/on/host<^^>:<^>/path/in/container<^^> ...
* An **anonymous volume** is useful for when you would rather have Docker handle where the files are stored. It can be difficult, however, to refer to the same volume over time when it is an anonymous volumes. To create an anonymous volume: 
    
        docker run -v <^>/path/in/container<^^> ...
* A **named volume** is similar to an anonymous volume. Docker manages where on disk the volume is created, but you give it a volume name. To create a named volume: 
    
        docker volume create <^>somevolumename<^^>
        docker run -v <^>name<^^>:<^>/path/in/container<^^> ...
