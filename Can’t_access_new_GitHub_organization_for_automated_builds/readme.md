---
type: kbase
version: 3
title: "Can’t access new GitHub organization for automated builds"
summary: "After creating an Automated Build from a GitHub organization and making changes, the settings aren't correct, and the GitHub organization is not accessible from Docker Hub. On the Authorize Application page, make sure that all of the organizations you need access to have a green check mark, otherwise click the Request Access button. Also, if you're using a GitHub organization, your profile on your organization's people page (https://github.com/orgs/**your-org**/people) has to be set to public."
dateCreated: "Tue, 08 Dec 2015 02:17:19 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 3
internal: no
author: "sabin.basyal"
platform:
testedon:
tags:
product:
  - Hub
uniqueid: KB000207
---

TESTING TESTING TESTING AND MORE TESTING!!!

### Overview

After creating an Automated Build from a GitHub organization and making changes, the settings aren't correct, and the GitHub organization is not accessible from Docker Hub.

### Scenario

Created a new organization on GitHub  
Created a repository inside the new organization

In Docker Hub:  
- GitHub account is linked to Docker Hub for Automated Builds  
- A new repo is created for Automated Builds

Issue: can't see that new organization

### Resolution

You may need to revoke the GitHub access and then request access on your repos when relinking.

You should do the following:

1. Unlink your GitHub account at Docker Hub: [https://hub.docker.com/account/authorized-services/](https://hub.docker.com/account/authorized-services/ "https://hub.docker.com/account/authorized-services/")
2. Revoke the **Docker Hub Registry** application on your GitHub account: [https://github.com/settings/applications
    26](https://github.com/settings/applications)
3. Link to your GitHub account again using **Public & Private Access**: [https://hub.docker.com/account/authorized-services/](https://hub.docker.com/account/authorized-services/ "https://hub.docker.com/account/authorized-services/")
4. On the **Authorize Application** page, make sure that all of the organizations you need access to have a green check mark, otherwise click the **Request Access** button.

Note: There has been a change with the GitHub API, so public and private access is required at the moment. We do have an internal ticket open to fix the **limited** access.

Also, if you're using a GitHub organization, your profile on your organization's people page ([https://github.com/orgs/**your-org**/people](https://github.com/orgs/**your-org**/people "https://github.com/orgs/**your-org**/people")) has to be set to public.
