---
type: kbase
version: 3
title: "What are the current resource limits placed on Automated Builds?"
summary: "Question What are the current resource limits placed on Automated Builds? Answer The current limits placed on Automated Builds are: 2 hours 2 GB RAM 1 CPU 30 GB Disk Space For larger builds you should either break them into several Automated Builds connected by FROM statements and Repository Links, or build them locally on your machine and push them to Docker Hub."
dateCreated: "Wed, 09 Dec 2015 15:22:10 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "nhazlett"
platform:
testedon:
tags:
product:
  - EE
---

## Question

What are the current resource limits placed on Automated Builds?

## Answer

The current limits placed on Automated Builds are:

* 2 hours
* 2 GB RAM
* 1 CPU
* 30 GB Disk Space

For larger builds you should either break them into several Automated Builds connected by `**FROM**`statements and Repository Links, or build them locally on your machine and push them to Docker Hub.