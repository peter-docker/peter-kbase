---
type: kbase
version: 3
title: "Is there a way to orchestrate the order of service deployment in a stack in Docker Cloud?"
summary: "Question Is there a way to orchestrate the order of service deployment in a stack in Docker Cloud? Answer You should be able to get the control you need using the sequential setting within the services wizard when creating your service. Documentation referencing sequential deployment and scaling can be found here."
dateCreated: "Fri, 20 May 2016 21:05:51 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "nhazlett"
platform:
testedon:
tags:
  - Docker Cloud
product:
  - Hub
---

## Question

Is there a way to orchestrate the order of service deployment in a stack in Docker Cloud?

## Answer

You should be able to get the control you need using the sequential setting within the services wizard when creating your service. Documentation referencing sequential deployment and scaling can be found [here](https://docs.docker.com/docker-cloud/apps/service-scaling/#when-should-i-use-sequential-scaling "https://docs.docker.com/docker-cloud/apps/service-scaling/#when-should-i-use-sequential-scaling").
