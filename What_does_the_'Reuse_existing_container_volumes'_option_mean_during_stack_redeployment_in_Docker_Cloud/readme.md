---
type: kbase
version: 2
title: "What does the 'Reuse existing container volumes?' option mean during stack redeployment in Docker Cloud?"
summary: "Data volumes are volumes which are mounted from the host itself whereas data volume containers mount the volume(s) from a volume container (e.g. The test-mysql MySQL container will always use the /var/lib/mysql volume mounted on the test-busybox Busybox container. option is set to ‘No’ during stack redeployment, the volume on the test-busybox container will be reset and all data on /var/lib/mysql will be gone."
dateCreated: "Fri, 22 Apr 2016 04:15:59 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 4
internal: no
author: "kenny.lim"
platform:
testedon:
tags:
product:
---

## Question

What does the "Reuse existing container volumes?" option mean during stack redeployment in Docker Cloud?

## Answer

There are two types of volumes that you can mount on a container which are:

* Data volumes
* Data volume containers

Data volumes are volumes which are mounted from the host itself whereas data volume containers mount the volume(s) from a volume container (e.g. busybox container). Take note that you would need to use the 
**volume_from**key instead of the usual 
**volume**key in order to use data volume containers' volume(s).

For more information on this, please refer to the documentation provided in the link below:  

[https://docs.docker.com/docker-cloud...rence/#volumes](https://docs.docker.com/docker-cloud/feature-reference/stack-yaml-reference/#volumes "https://docs.docker.com/docker-cloud/feature-reference/stack-yaml-reference/#volumes")

Take the below Stackfile as an example:

    test-busybox:
      image: 'busybox:latest'
      command: /bin/echo
      restart: always
      roles:
        - global
      volumes:
        - /var/lib/mysql
    
    test-mysql:
      image: 'tutum/mysql:latest'
      environment:
        - MYSQL_PASS=admin
        - MYSQL_USER=admin
      expose:
        - '3306'
      restart: always
      roles:
        - global
      volumes_from:
        - test-busybox

The 
test-mysqlMySQL container will always use the 
/var/lib/mysqlvolume mounted on the 
test-busyboxBusybox container. If the "**Reuse existing container volumes?**" option is set to ‘**No**’ during stack redeployment, the volume on the 
test-busyboxcontainer will be reset and all data on 
/var/lib/mysqlwill be gone.

You can also replicate this if you use the following Stackfile:

    test-mysql:
      image: 'tutum/mysql:latest'
      environment:
        - MYSQL_PASS=admin
        - MYSQL_USER=admin
      expose:
        - '3306'
      restart: always
      roles:
        - global
      volumes:
        - /var/lib/mysql