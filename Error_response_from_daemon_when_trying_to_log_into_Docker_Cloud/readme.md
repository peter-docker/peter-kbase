---
type: kbase
version: 3
title: "Error response from daemon when trying to log into Docker Cloud"
summary: "Receiving an error when trying to log into Docker Cloud using CLI: Error response from daemon: Get https://index.docker.io/v1/users/: dial tcp: i/o timeout Open https://index.docker.io/v1/users/ in a browser after logging into Docker Cloud UI. Navigate to https://us-east-1-amazonaws.com/v1/users in a browser. Configured network settings to use the Google IP addresses 8.8.8.8 and 8.8.4.4 as the DNS servers."
dateCreated: "Wed, 07 Jun 2017 14:51:03 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "hannahagee"
platform:
testedon:
tags:
  - Docker Cloud
  - daemon
  - error
product:
  - Hub
---

## Issue

Receiving an error when trying to log into Docker Cloud using CLI:

```
$ docker login
Error response from daemon: Get https://index.docker.io/v1/users/: dial tcp: i/o timeout
```


## Diagnostics

* Try pinging `index.docker.io`:

```
$ ping index.docker.io
--- us-east-1-amazonaws.com ping statistics --- 
4 packets transmitted, 0 received, 100% packet loss, time 3053ms
```

* Open [https://index.docker.io/v1/users/](https://index.docker.io/v1/users/ "https://index.docker.io/v1/users/") in a browser after logging into Docker Cloud UI. You should received **ok**.
* Navigate to [https://us-east-1-amazonaws.com/v1/users](https://us-east-1-amazonaws.com/v1/users "https://us-east-1-amazonaws.com/v1/users") in a browser. It should return a **503 HTTP** status.
* Check <https://status.docker.com/> and <https://status.aws.amazon.com/> for any outage notifications.

## Resolution

* Configured network settings to use the Google IP addresses 8.8.8.8 and 8.8.4.4 as the DNS servers.
* Restart Docker daemon.
