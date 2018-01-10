---
type: kbase
version: 3
title: "Pull error ”The Repository is Locked, access denied'"
summary: "Why am I receiving the error”The Repository is Locked, access denied”message when trying to pull or push my Docker Hub repository? If you don't have any free private repositories available, then the new repository will become locked due to the visibility defaulting as Private. If you encounter such a problem, please change your default repository visibility to Public and push your image to a public repository."
dateCreated: "Wed, 09 Dec 2015 17:52:36 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 7
internal: no
author: "nhazlett"
platform:
testedon:
tags:
product:
  - Hub
---

## Question

Why am I receiving the error `”The Repository is Locked, access denied”` message when trying to pull or push my Docker Hub repository?

## Answer

This problem may occur for two reasons:

* User has their [Default Repository Visibility](https://hub.docker.com/account/settings/ "https://hub.docker.com/account/settings/") set to `Private` and they push a new repository using the Docker CLI. If you don't have any free private repositories available, then the new repository will become locked due to the visibility defaulting as `Private`.

* Decline in payment collection for the current subscription plan for the private repositories.

If you encounter such a problem, please change your default repository visibility to `Public` and push your image to a public repository. If you need the repository to remain private, confirm you have enough private repositories available and that yourbilling is current. If neither of these methods are relevant to your issue please send an email to [billing@docker.com](mailto:billing@docker.com "mailto:billing@docker.com").
