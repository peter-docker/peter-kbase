---
type: kbase
version: 3
title: "What happens to my security access labels when I upgrade UCP to 2.2.x as access control changes?"
summary: "When upgrading to UCP 2.2.x, some might be concerned about the work done to access controls in previous versions and how is this accomplished going forward. When new resources are created, ensure the /Shared/Legacy/<labelname> collection is selected in UCP so that your team has access to these resources. Refer to https://docs.docker.com/datacenter/u...ccess-control/ and the associated documents in the Access Control section of the documentation."
dateCreated: "Fri, 15 Sep 2017 15:58:40 GMT"
dateModified: "Wed, 04 Oct 2017 03:39:19 GMT"
dateModified_unix: 1507088359
upvote: 0
internal: no
author: "scasey"
platform:
testedon:
  - ucp-2.2.0
tags:
  - access control
  - upgrading
product:
  - EE
---

## Issue

When upgrading to UCP 2.2.x, some might be concerned about the work done to access controls in previous versions and how is this accomplished going forward. You will no longer see an option for teams to have resource labels. Instead, you will find collection, grants, and roles along with respective labels.

## Resolution

All access control is configured through *Collections* and *Grants* in UCP 2.2.x.

A *Collection* can be any group of resources - images, volumes, secrets, networks, etc.

*Grants* apply Roles to users/teams for each Collection.

All of the security labels in UCP 2.1.x have been migrated under the collection `/Shared/Legacy/<labelname>`.

Check the **Grants** section of the **User Management** page and ensure a grant exists for your team that applies a Role for the `/Shared/Legacy/<labelname>` collection.

When new resources are created, ensure the `/Shared/Legacy/<labelname>` collection is selected in UCP so that your team has access to these resources.

Refer to [https://docs.docker.com/datacenter/u...ccess-control/](https://docs.docker.com/datacenter/ucp/2.2/guides/access-control/ "https://docs.docker.com/datacenter/ucp/2.2/guides/access-control/") and the associated documents in the **Access Control** section of the documentation.