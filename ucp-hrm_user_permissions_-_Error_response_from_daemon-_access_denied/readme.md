---
type: kbase
version: 13
title: "ucp-hrm user permissions - Error response from daemon: access denied"
summary: "Since the ucp-hrm network is external to this compose stack (as shown the following compose file), it will not be created by docker-compose and access to it must be granted through the UI. Since the ucp navigate to User Management > Teams on the UCP UI, choose a team you want to give access to the ucp-hrm network, then click on the Permissions tab, and add the below label:"
dateCreated: "Thu, 17 Aug 2017 18:56:45 GMT"
dateModified: "Wed, 04 Oct 2017 03:55:24 GMT"
dateModified_unix: 1507089324
upvote: 0
internal: no
author:
  - fotios.tsiadimos
  - adamancini
platform:
testedon:
  - ucp-2.0.0
  - ucp-2.1.0
  - ucp-2.1.5
tags:
  - access control
  - networking
  - error
product:
  - EE
---

When trying to schedule containers that use hostnamed-based routing (HRM), you might receive the following error message:

    Error response from daemon: access denied

## Prerequisites

* UCP >= 2.0.x < 2.2.x
* Docker Engine >= CSE 1.12.z

[https://success.docker.com/Policies/Compatibility_Matrix](https://success.docker.com/Policies/Compatibility_Matrix "https://success.docker.com/Policies/Compatibility_Matrix")

## Use Case

A restricted user attempts to deploy a stack or service that uses an external network, like ucp-hrm, using access labels. They supply a docker-compose.yml file similar to the one below and get Access Denied messages from UCP. Every resource in UCP 2.0 needs a label to grant access to it. *Any external resources **must** have a label applied to them describing who has access*.

Below is an example set of teams with various levels of access to swarm resources.

| Team \ Environment | alpha-dev | alpha-staging | alpha-production | beta-dev | beta-staging | beta-production |
|---|---|---|---|---|---|---|
| Dev Team Alpha | Restricted Control | View | View | - | - | - |
| Dev Team Beta | - | - | - | Restricted Control | View | - |
| DevOps | View | Restricted Control | Restricted Control | View | Restricted Control | View |
| DevOps (sensitive) | - | - | - | - | - | Restricted Control |

For example, if a member of `DevTeam-Alpha` were to attempt to deploy this stack, they might encounter an `Error response from daemon: access denied` message in the UI.

Since the ucp-hrm network is external to this compose stack (as shown the following compose file), it will not be created by docker-compose and access to it must be granted through the UI.

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
          - ucp-hrm
        volumes:
          - static_content:/var/www/html:ro
        deploy:
          mode: replicated
          replicas: 1
          resources:
            limits:
              memory: 1G
          labels:
            - com.docker.ucp.access.label=alpha-dev
            - com.docker.ucp.mesh.http.80=external_route=http://${DOMAIN},redirect=https://${DOMAIN}
            - com.docker.ucp.mesh.http.443=external_route=sni://${DOMAIN},internal_port=443
    networks:
      ucp-hrm:
        external: true
      frontend:
        labels:
          com.docker.ucp.access.label=alpha-dev
    
    volumes:
      static_content:
        labels:
          com.docker.ucp.access.label=alpha-dev

## Resolution

Since the ucp navigate to **User Management > Teams** on the UCP UI, choose a team you want to give access to the ucp-hrm network, then click on the **Permissions** tab, and add the below label:

* **ucp-hrm** --> Restricted Control

![](./images/Selection_135.png)
