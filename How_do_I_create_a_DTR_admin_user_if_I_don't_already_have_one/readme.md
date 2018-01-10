---
type: kbase
version: 5
title: "How do I create a DTR admin user if I don't already have one?"
summary: "CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES 1ce30a0943a0 docker/trusted-registry-garant:1.4.3 'garant /config/garan' 2 hours ago Up 2 hours docker_trusted_registry_auth_server 923f02f750c0 docker/trusted-registry-log-aggregator:1.4.3 'log-aggregator' 2 hours ago Up 2 hours docker_trusted_registry_log_aggregator 036c069ed1f0 docker/trusted-registry-admin-server:1.4.3 'server' 2 hours ago Up 2 hours 80/tcp docker_trusted_registry_admin_server df12e70aaf35 docker/trusted-registry-nginx:1…"
dateCreated: "Fri, 18 Sep 2015 15:03:13 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 1
internal: no
author: "justinnevill"
platform:
  - linux
testedon:
tags:
product:
  - EE
---

### Overview

When installing Docker Trusted Registry it's possible to create an admin user that does not have admin rights. This will prevent you from logging into the admin web interface. You can recover from this by running a special recovery container.

### Symptoms

You can't log into your DTR web interface.

### Diagnostic Steps

1. Point your browser to http://<^><dtr-host-ip><^^>/
2. Try logging in with the user you created during installation.
3. Access is denied.
4. However, you can confirm that the DTR admin container is still running by executing the following command on the DTR host: 
    
        docker ps
    
    You should see output similar to the following with the status for `trusted-registry` containers showing that they are up and running:
    
        CONTAINER ID        IMAGE                                          COMMAND                  CREATED             STATUS                          PORTS                                      NAMES
        1ce30a0943a0        docker/trusted-registry-garant:1.4.3           "garant /config/garan"   2 hours ago         Up 2 hours                                                                 docker_trusted_registry_auth_server
        923f02f750c0        docker/trusted-registry-log-aggregator:1.4.3   "log-aggregator"         2 hours ago         Up 2 hours                                                                 docker_trusted_registry_log_aggregator
        036c069ed1f0        docker/trusted-registry-admin-server:1.4.3     "server"                 2 hours ago         Up 2 hours                      80/tcp                                     docker_trusted_registry_admin_server
        df12e70aaf35        docker/trusted-registry-nginx:1.4.3            "watcher"                2 hours ago         Up 2 hours                      0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   docker_trusted_registry_load_balancer
        9641ff478ab7        docker/trusted-registry-index:1.4.3            "server"                 2 hours ago         Up 2 hours                                                                 docker_trusted_registry_registry_index
        f99e64c70105        postgres:9.4.1                                 "/docker-entrypoint.s"   2 hours ago         Up 2 hours                      5432/tcp                                   docker_trusted_registry_postgres

### Resolution

To solve this problem, log into the DTR admin web interface using an ambassador container, create an admin user with admin rights, and then stop the ambassador container.

1. Make sure you are logged into the host that is running the DTR admin container.
2. Make sure you are logged in as a user that's a member of the`docker`group, or have root privileges. Otherwise, you may have to add the prefix `sudo` to the `docker run` command below.
3. You can run an [ambassador container](https://docs.docker.com/engine/admin/ambassador_pattern_linking/ "https://docs.docker.com/engine/admin/ambassador_pattern_linking/") to get temporary, insecure access to your DTR instance by executing the following command: 
    
        docker run --rm -it --link docker_trusted_registry_admin_server:admin -p 9999:80 svendowideit/ambassador
4. This will give you access on port 9999 to your DTR instance:`http://<^><dtr-host-up><^^>:9999/admin/`.
5. Make sure you stop the ambassador container once you've given your user correct permissions.
