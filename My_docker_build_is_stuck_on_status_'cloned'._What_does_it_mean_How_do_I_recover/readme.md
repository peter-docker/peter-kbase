---
type: kbase
version: 2
title: "My docker build is stuck on status 'cloned'. What does it mean? How do I recover?"
summary: "The 'cloned' status in Docker Hub is one of the states an Automated Build could be in throughout the entire build process. It means that the builder is currently in the process of cloning your source repository (from either GitHub or BitBucket) to the build container. The duration of this process varies according to the size of your source repository: i.e., if your repository is large, say 500MB, it might take a while to clone the source repository in the build container."
dateCreated: "Tue, 08 Dec 2015 06:40:19 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "kenny.lim"
platform:
testedon:
tags:
product:
  - Hub
---

## Question

My docker build is stuck on status "cloned". What does it mean? How do I recover?

## Answer

The "cloned" status in Docker Hub is one of the states an Automated Build could be in throughout the entire build process. It means that the builder is currently in the process of cloning your source repository (from either GitHub or BitBucket) to the build container. The duration of this process varies according to the size of your source repository: i.e., if your repository is large, say 500MB, it might take a while to clone the source repository in the build container.

Additionally, automated build jobs will automatically fail if the job takes longer than 2 hour to complete.