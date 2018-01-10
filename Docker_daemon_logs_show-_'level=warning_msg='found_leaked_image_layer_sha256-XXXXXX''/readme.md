---
type: kbase
version: 3
title: "Docker daemon logs show: 'level=warning msg='found leaked image layer sha256:XXXXXX''"
summary: "Jun 06 14:05:02 docker-ucp1 dockerd[10956]: time='2017-06-06T14:05:02.309393708-04:00' level=warning msg='found leaked image layer sha256:398edd8549bb0dda3213247f7ea774e55afbcb19c205ca53ad9c17c8b756ebf5' Jun 06 14:05:02 docker-ucp1 dockerd[10956]: time='2017-06-06T14:05:02.309414653-04:00' level=warning msg='found leaked image layer sha256:c64033084c13c4f6d8d95310a4b744ba250a28bbf97c7883582b838ddbdb2a00' Jun 06 14:05:02 docker-ucp1 dockerd[10956]: time='2017-06-06T14:05:02.309425882-04:00' leve…"
dateCreated: "Tue, 06 Jun 2017 18:08:22 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "squizzi"
platform:
  - linux
testedon:
tags:
  - logging
  - daemon
  - error
product:
  - EE
---

## Issue

Docker daemon logs exhibit the following error during normal usage:

    Jun 06 11:38:08 examplehost dockerd[8264]: time="2017-06-02T11:38:08.613115484-07:00" level=warning msg="found leaked image layer sha256:f98f0841bf84b854fdb01f69f92e9640ca6e11156ah7d4aa3f65c237b5e1e7az"
    Jun 06 11:38:08 examplehost dockerd[8264]: time="2017-06-02T11:38:08.613147070-07:00" level=warning msg="found leaked image layer sha256:c26d41a4dc960f079be0c34b5f32a2ccc574e59b748a104e204c157c4f0c31ba"

## Explanation

If using Universal Control Plane, the above messages are **normal** and can safely be ignored.

These messages are caused by UCP issuing the `docker system df` command, which is often run by UCP to provide size and consumption of images, containers, and volume metrics within the UI. UCP regularly runs this command on each host.

## Observe the Behavior

To observe the behavior that triggers the messages on Linux, perform the following steps:

1. Open two terminals.
2. In the first, run: 
    
        # journalctl -fu docker
3. In the second terminal, run the following command and observe the `journalctl` output produced: 
    
        # docker system df
4. You should see the following output in the first terminal, where `journalctl` is running: 
    
        Jun 06 14:05:02 docker-ucp1 dockerd[10956]: time="2017-06-06T14:05:02.309393708-04:00" level=warning msg="found leaked image layer sha256:398edd8549bb0dda3213247f7ea774e55afbcb19c205ca53ad9c17c8b756ebf5"
        Jun 06 14:05:02 docker-ucp1 dockerd[10956]: time="2017-06-06T14:05:02.309414653-04:00" level=warning msg="found leaked image layer sha256:c64033084c13c4f6d8d95310a4b744ba250a28bbf97c7883582b838ddbdb2a00"
        Jun 06 14:05:02 docker-ucp1 dockerd[10956]: time="2017-06-06T14:05:02.309425882-04:00" level=warning msg="found leaked image layer sha256:e8dfc45487fc9b8b8d92137ed700b80a0ebd40a0a636e7e0b33b5599b814e508"
        Jun 06 14:05:02 docker-ucp1 dockerd[10956]: time="2017-06-06T14:05:02.309440662-04:00" level=warning msg="found leaked image layer sha256:481fd1c7cfd86c59b1c75943ea5659cda3742270e07b77dee769fa20d2bff12a"
