---
type: kbase
version: 5
title: "Why can't I create automated builds for my organization?"
summary: "You're unable to create an automated build from GitHub for an organization you're a member of. On the Authorize Application, page make sure that all of the organizations you need access to have a green check mark. NOTE: Public and private access is required at the moment because of a change with the GitHub API. If you're using a GitHub organization, your profile on your organization's people page (https://github.com/orgs/<your-org>/people) must be set to public."
dateCreated: "Fri, 25 Mar 2016 17:21:19 GMT"
dateModified: "Mon, 06 Mar 2017 02:14:29 GMT"
dateModified_unix: 1488766469
upvote: 3
internal: no
author: "kizbitz"
platform:
testedon:
tags:
product:
  - Hub
---

# Issue

You're unable to create an automated build from GitHub for an organization you're a member of.

# Possible Causes

* GitHub link is corrupted.
* Your profile on your GitHub organization's people page is not set to public.

# Resolution

To unlink and relink to Github:

* Unlink your GitHub account at Docker Hub: <https://hub.docker.com/account/authorized-services/>
* Revoke the **Docker Hub Registry** application on your account at github.com: <https://github.com/settings/applications>
* Link to your GitHub account again using **Public & Private Access**: <https://hub.docker.com/account/authorized-services/>
* On the **Authorize Application**, page make sure that all of the organizations you need access to have a green check mark. Otherwise click the **Request Access** button.

**NOTE**: Public and private access is required at the moment because of a change with the GitHub API. We do have an internal ticket open to fix the **limited** access.

If you're using a GitHub organization, your profile on your organization's people page (https://github.com/orgs/<^><your-org><^^>/people) must be set to **public**.
