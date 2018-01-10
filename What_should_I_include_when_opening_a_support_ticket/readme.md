---
type: kbase
version: 15
title: "What should I include when opening a support ticket?"
summary: "To assist you better, it is recommended that you provide a short summary of your Docker use case as well as a detailed description of the issue you are facing. For Docker EE, this can be done by using a UCP support dump, which is an archive that contains UCP system logs and diagnostic information. If the user interface is not accessible, you may perform the following steps instead to obtain a single-node version of the UCP support dump:"
dateCreated: "Tue, 08 Dec 2015 06:21:16 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 4
internal: no
author: "kenny.lim"
platform:
testedon:
tags:
product:
  - EE
---

To assist you better, it is recommended that you provide a short summary of your Docker use case as well as a detailed description of the issue you are facing.

Additionally, it helps speed the troubleshooting process when information about your Docker configuration and host environment is provided. For Docker EE, this can be done by using a UCP support dump, which is an archive that contains UCP system logs and diagnostic information. To obtain a support dump:

1. Log into the UCP UI with an administrator account.
2. On the top-right menu, click your username, and choose **Support Dump**. An archive will be downloaded via your browser after a brief time interval.

If the user interface is not accessible, you may perform the following steps instead to obtain a single-node version of the UCP support dump:

1. Obtain direct CLI access to the Docker daemon on a UCP manager node.
2. On Linux nodes run the CLI support tool with the following command: 
    
        docker container run --rm  \
          --name ucp --log-driver none \
          -v /var/run/docker.sock:/var/run/docker.sock \
          docker/ucp:2.2.2 \
          support  > docker-support-$HOSTNAME.tgz
    On Windows worker nodes, run the following command to generate a local support dump: 
    
        PS> docker run --name windowssupport -v 'C:\ProgramData\docker\daemoncerts:C:\ProgramData\docker\daemoncerts' -v 'C:\Windows\system32\winevt\logs:C:\eventlogs:ro' docker/ucp-dsinfo-win; docker cp windowssupport:'C:\dsinfo' .; docker rm -f windowssupport
    This command creates a directory named `dsinfo` in your current directory. If you want an archive file, you need to create it from the `dsinfo` directory.