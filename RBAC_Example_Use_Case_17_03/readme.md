---
type: guide
version: 10
title: "RBAC Example Use Case (Docker 17.03)"
summary: "This guide is an example of how a team of engineers might use RBAC for their projects and how each concept relates to their use case."
dateCreated: "Tue, 16 May 2017 16:06:08 GMT"
dateModified: "Wed, 08 Nov 2017 15:41:51 GMT"
dateModified_unix: 1510173667
internal: no
author: "programmerq"
platform:
testedon:
  - ee-17.03.0-ee-1
  - ucp-2.1.0
tags:
  - access control
product:
  - EE
---

## Introduction

Role-Based Access Control (RBAC) in UCP is covered in the [Docker documentation](https://docs.docker.com/datacenter/ucp/2.1/guides/ "https://docs.docker.com/datacenter/ucp/2.1/guides/").

This guide expands on the concepts discussed in the documentation by using an example use case of how a team of engineers might use RBAC for their projects.

> **Note:** This guide covers RBAC in Docker EE 17.03 and UCP 2.1. For Docker EE 17.06 and UCP 2.2, refer to [RBAC Example Use Case](/article/RBAC_Example-Overview).

## Use Case

The use case discussed in this guide consists of two developer groups and one devops group. Individual member of this team might want to deploy various applications. There are two example "projects." One is called "project alpha," and the other is called "project beta." In production, "project beta" has some sensitive information that should not be accessed by the developers. Instead, only the head of the devops group should be allowed to access that environment.

## Users

Users for this team are as follows. Each person needs a UCP user account. All accounts are created with a default access of "no access."

* billy
* alice
* chuck
* dave
* leonard
* earl
* frank
* gertrude

## Teams

Users are then organized into teams. There are two development teams. Leonard is on both teams. Technically, there is only one devops team, but only Gertrude is authorized to handle containers that have sensitive data. Because of this, creating a new team with just Gertrude in it allows Gertrude to have a different level of access.

* Dev Alpha 
    * billy
    * alice
    * leonard
* Dev Beta 
    * chuck
    * dave
    * leonard
* DevOps 
    * earl
    * frank
    * gertrude
* DevOps (sensitive) 
    * gertrude

The following shows how the Dev Team Alpha looks inside the UCP user interface:

![](./images/teamscreenshot2.png)

## Labels

Each deployed unit of services gets a label. That label is unique across the entire UCP installation. There is a dev, staging, and production deploy for each project: alpha and beta. The beta project handles sensitive data.

* alpha-dev
* alpha-staging
* alpha-production
* beta-dev
* beta-staging
* beta-production

## Permissions

This image illustrates RBAC permissions:  

![](./images/permissions_ucp.png)

The following table shows how each of these permissions map to each environment for each team. For example, the dev team for the alpha project should have access to the alpha-dev environment and be able to do deploys. No one has "full control" permissions because no on needs access to the kernel, and no one needs to be able to run privileged containers. The members of the "DevOps (sensitive)" team are all also on the DevOps team, so it is only necessary to add the one permission difference that is needed. If a person is on multiple teams, the highest permission that is granted for a given resource label is applied.

| Team \ Environment | alpha-dev          | alpha-staging      | alpha-production   | beta-dev           | beta-staging       | beta-production    |
|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Dev Team Alpha     | Restricted Control | View               | View               | -                  | -                  | -                  |
| Dev Team Beta      | -                  | -                  | -                  | Restricted Control | View               | -                  |
| DevOps             | View               | Restricted Control | Restricted Control | View               | Restricted Control | View               |
| DevOps (sensitive) | -                  | -                  | -                  | -                  | -                  | Restricted Control |

Now, if anyone tries to access a resource that they have only "view" permissions to, they will get an "access denied" error message. If they don't have any permissions for a resource at all, the resource will not even show up. It will be like it does not exist.

If any of these users want to deploy a resource, they **must** select a label for that new resource, or they will get an error because they did not specify a label. Any pre-existing resources **must** have a label applied to them describing who has access. An example Docker Compose file looks like this:

    ---
    version: '3.1'
    
    services:
      demodocker:
        image: nginx:alpine
        environment:
          - DOMAIN=test.ucp.local
        ports:
          - 80
          - 443
        networks:
          - frontend
        volumes:
          - static_content:/var/www/html:ro
        deploy:
          mode: replicated
          replicas: 1
          resources:
            limits:
              memory: 1G
          <^>labels:<^^>
            <^>- com.docker.ucp.access.label=alpha-dev<^^>
    
    networks:
      frontend:
        <^>labels:<^^>
          <^>com.docker.ucp.access.label=alpha-dev<^^>
    
    volumes:
      static_content:
        <^>labels:<^^>
          <^>com.docker.ucp.access.label=alpha-dev<^^>
