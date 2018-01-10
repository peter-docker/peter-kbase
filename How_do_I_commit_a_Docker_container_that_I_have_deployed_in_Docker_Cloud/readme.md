---
type: kbase
version: 3
title: "How do I commit a Docker container that I have deployed in Docker Cloud?"
summary: "A container was recently deployed on Docker Cloud and some changes have been made to it. How can these changes in the container be committed to a new Docker image? This feature is not currently available on Docker Cloud as it's generally considered bad practice to commit running images to create new images, since there is no record of what was done within the image. We recommend either building the image locally on your own node or building them using the Automated Build service on Docker Cloud."
dateCreated: "Fri, 18 Mar 2016 08:38:11 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "kenny.lim"
platform:
testedon:
tags:
  - Docker Cloud
product:
  - Hub
---

## Question

A container was recently deployed on Docker Cloud and some changes have been made to it. How can these changes in the container be committed to a new Docker image?

## Answer

This feature is not currently available on Docker Cloud as it's generally considered bad practice to commit running images to create new images, since there is no record of what was done within the image. We recommend either building the image locally on your own node or building them using the Automated Build service on Docker Cloud. You can read more about Docker Cloud's Automated Build features [here](https://docs.docker.com/docker-cloud/feature-reference/automated-build/ "https://docs.docker.com/docker-cloud/feature-reference/automated-build/").
