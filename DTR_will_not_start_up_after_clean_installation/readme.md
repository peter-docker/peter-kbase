---
type: kbase
version: 7
title: "DTR will not start up after clean installation"
summary: "CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES 1ce30a0943a0 docker/trusted-registry-garant:1.4.3 'garant /config/garan' 2 hours ago Up 2 hours docker_trusted_registry_auth_server 923f02f750c0 docker/trusted-registry-log-aggregator:1.4.3 'log-aggregator' 2 hours ago Up 2 hours docker_trusted_registry_log_aggregator 036c069ed1f0 docker/trusted-registry-admin-server:1.4.3 'server' 2 hours ago Restarting (1) 20 minutes ago 80/tcp docker_trusted_registry_admin_server df12e70aaf35 docker/trust…"
dateCreated: "Fri, 04 Mar 2016 20:08:45 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "pvnovarese"
platform:
  - linux
testedon:
  - dtr-1.4.3
tags:
  - installing
  - error
product:
  - EE
---

### Overview

After installing DTR 1.4.3, when trying accessing the web interface, an nginx error message **502 Bad Gateway** is seen.

### Symptoms

* Web interface gives **502 Bad Gateway** error message
* `docker_trusted_registry_admin_server` container isn't running

### Diagnostic Steps

Verify that `docker_trusted_registry_admin_server` isn't running and fails to write to syslog.

1. Check the running containers by running: 
    
        docker ps
    The output will look similar to the following: 
    
        CONTAINER ID        IMAGE                                          COMMAND                  CREATED             STATUS                          PORTS                                      NAMES
        1ce30a0943a0        docker/trusted-registry-garant:1.4.3           "garant /config/garan"   2 hours ago         Up 2 hours                                                                 docker_trusted_registry_auth_server
        923f02f750c0        docker/trusted-registry-log-aggregator:1.4.3   "log-aggregator"         2 hours ago         Up 2 hours                                                                 docker_trusted_registry_log_aggregator
        036c069ed1f0        docker/trusted-registry-admin-server:1.4.3     "server"                 2 hours ago         
        Restarting (1) 20 minutes ago   80/tcp                                     docker_trusted_registry_admin_server
        df12e70aaf35        docker/trusted-registry-nginx:1.4.3            "watcher"                2 hours ago         Up 2 hours                      0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   docker_trusted_registry_load_balancer
        9641ff478ab7        docker/trusted-registry-index:1.4.3            "server"                 2 hours ago         Up 2 hours                                                                 docker_trusted_registry_registry_index
        f99e64c70105        postgres:9.4.1                                 "/docker-entrypoint.s"   2 hours ago         Up 2 hours                      5432/tcp                                   docker_trusted_registry_postgres
2. The `Restarting` status for the `docker_trusted_registry_admin_server` is a problem. Check the logs for that container with the following command: 
    
        docker logs docker_trusted_registry_admin_server
    The output will be: 
    
        INFO  [1.4.3] Initializing with an invalid license
        FATAL [1.4.3] Failed to create syslog logger error="Unix syslog delivery error"

### Resolution

The `Initializing with an invalid license` info message is expected since there's no license applied during installation (you have to log into the web interface to do this). However, DTR requires syslogd, and when it tries to log this info message and sees there is no syslogd running, it exits. This problem is usually encountered in minimal host OS environments (e.g. boot2docker) where syslog isn't running by default.

This issue can be resolved in either of the following ways:

1. Start syslogd. Consult your host OS documentation.

or

2. Manually load the license file by copying it to `/usr/local/etc/dtr/license.json`
