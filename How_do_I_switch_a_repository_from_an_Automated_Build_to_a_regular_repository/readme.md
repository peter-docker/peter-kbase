---
type: kbase
version: 3
title: "How do I switch a repository from an Automated Build to a regular repository?"
summary: "How do I switch a repository from an Automated Build to a regular repository? The only way to take a repository that is not an automated build and make it into a regular repository is to delete the automated build repo and then re-create the repository as a normal one. To delete the build, log in to hub.docker.com and click on the 'Details' for the automated build you'd like to delete."
dateCreated: "Thu, 28 Jan 2016 18:09:56 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 1
internal: no
author: "programmerq"
platform:
testedon:
tags:
product:
  - EE
---

## Question

How do I switch a repository from an Automated Build to a regular repository?

## Answer

The only way to take a repository that is not an automated build and make it into a regular repository is to delete the automated build repo and then re-create the repository as a normal one.

You can pull all the tags associated with this repository, remove the automated build, and then re-push everything.

To delete the build, log in to [hub.docker.com](http://hub.docker.com/ "http://hub.docker.com/") and click on the "Details" for the automated build you'd like to delete. Next, click on "Settings". Finally, choose the "Delete" option.