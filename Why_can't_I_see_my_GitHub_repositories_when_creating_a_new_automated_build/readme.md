---
type: kbase
version: 5
title: "Why can't I see my GitHub repositories when creating a new automated build?"
summary: "If you're unable to see/select your GitHub repositories when attempting to create an automated build, the possible causes include the following: NOTE: If you selected 'Limited Access' when linking to your GitHub account, you need to trigger your build manually or create a GitHub webhook. Also, if you're using a GitHub organization, your profile on your organization's people page (https://github.com/orgs/<your-org>/people) has to be set to public."
dateCreated: "Wed, 16 Mar 2016 18:42:48 GMT"
dateModified: "Mon, 12 Dec 2016 19:45:26 GMT"
dateModified_unix: 1481571926
upvote: 2
internal: no
author: "kizbitz"
platform:
testedon:
tags:
product:
  - Hub
---

If you're unable to see/select your GitHub
repositories when attempting to create an automated build, the possible causes include the following:

* Your GitHub link is corrupted.
* The profile for your GitHub organization's people page is not set to public.

# Resolution

The following steps should resolve the problem:

* Unlink your GitHub account at Docker Hub: <https://hub.docker.com/account/authorized-services/>
* Revoke the **Docker Hub Registry** application on your account at github.com: <https://github.com/settings/applications>
* Link to your GitHub account again using **Public & Private Access**: <https://hub.docker.com/account/authorized-services/>
* On the **Authorize Application** page, make sure that all of the organizations you need access to have a green check mark. Otherwise click the **Request Access** button.

NOTE: If you selected "Limited Access" when linking to your GitHub account, you need to trigger your build manually or create a GitHub webhook. Details can be found at [https://docs.docker.com/docker-hub/github/#auto-builds-and-limited-linked-github-accounts](https://docs.docker.com/docker-hub/github/#auto-builds-and-limited-linked-github-accounts "https://docs.docker.com/docker-hub/github/#auto-builds-and-limited-linked-github-accounts")

Also, if you're using a GitHub organization, your profile on your organization's people page (https://github.com/orgs/<^><your-org><^^>/people) has to be set to public.
