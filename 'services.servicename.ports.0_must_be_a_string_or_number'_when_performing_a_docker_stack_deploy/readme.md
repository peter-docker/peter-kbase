---
type: kbase
version: 3
title: "'services.servicename.ports.0 must be a string or number' when performing a docker stack deploy"
summary: "If you are attempting to configure a specific networking mode, such as host mode, you will need to use the docker service update command following deployment to perform the port declaration. For example, after deploying a web service where the target port is 80 and the published port is 8001, the syntax to update the mode from the default of ingress to host mode would be:"
dateCreated: "Thu, 20 Jul 2017 19:11:44 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "squizzi"
platform:
testedon:
  - ee-17.03.0-ee-1
tags:
product:
  - EE
---

## Issues

* When specifying ports using the long mode syntax the following error is observed:

    # docker stack deploy -c docker-compose.yml example
    services.web.ports.0 must be a string or number

* I am unable to configure host mode networking in a compose file, ingress is always set as the `PublishMode` following a `docker stack deploy`.

# Prerequisites

This issue effects the Docker Engine in Docker EE 17.03 and lower.

# Steps

If you are interested in configuring a different networking mode, do note that there is currently no way to specify networking modes in a Docker Compose v3 file. The only way to specify host mode networking is to utilize compose v3.2, which is only supported in Docker Engine 17.04 and higher.

If you are running Docker Engine 17.04 or higher, ensure you've configured the `version:` section of the compose file to:`version: 3.2`.

### Workarounds

* Use short mode syntax in compose files for port declaration, for example:

    services:
      nginx:
        image: nginx:latest
        deploy:
          replicas: 1
          placement:
            constraints: [node.role == worker]
        ports:
          - target: 80
            published: 8001
            protocol: tcp
        networks:
          - nginx_frontend
    
    networks:
       nginx_frontend:

would be:

    services:
      nginx:
        image: nginx:latest
        deploy:
          replicas: 1
          placement:
            constraints: [node.role == worker]
        ports:
          - "8001:80"
        networks:
          - nginx_frontend

* If you are attempting to configure a specific networking mode, such as host mode, you will need to use the `docker service update` command following deployment to perform the port declaration. For example, after deploying a web service where the target port is 80 and the published port is 8001, the syntax to update the mode from the default of ingress to host mode would be:

    docker service update example_web --publish-rm 8001:80 --publish-add "mode=host,target=80,published=8001"