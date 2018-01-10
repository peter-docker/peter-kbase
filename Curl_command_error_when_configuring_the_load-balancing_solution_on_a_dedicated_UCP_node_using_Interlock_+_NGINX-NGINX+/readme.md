---
type: kbase
version: 6
title: "Curl command error when configuring the load-balancing solution on a dedicated UCP node using Interlock + NGINX/NGINX+"
summary: "When configuring the load-balancing solution on a dedicated UCP node using Interlock + NGINX/NGINX+, incorrect curl syntax will cause errors similar to the following: curl: (6) Couldn't resolve host '=' curl: (6) Couldn't resolve host '' curl: (6) Couldn't resolve host 'dockerURL' curl: (6) Couldn't resolve host '=' curl: (1) Protocol 'tcp' not supported or disabled in libcurl"
dateCreated: "Wed, 30 Nov 2016 21:06:02 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
testedon:
tags:
  - error
product:
  - EE
---

### Overview

When configuring the load-balancing solution on a dedicated UCP node using Interlock + NGINX/NGINX+, incorrect curl syntax will cause errors similar to the following:

    curl: (6) Couldn't resolve host '='
    curl: (6) Couldn't resolve host ''
    curl: (6) Couldn't resolve host 'dockerURL'
    curl: (6) Couldn't resolve host '='
    curl: (1) Protocol "tcp" not supported or disabled in libcurl

### Diagnostic Steps

Proper syntax can be found in [Reference Architecture: Service Discovery and Load-Balancing with Docker Universal Control Plane (UCP)](https://success.docker.com/Architecture/Docker_Reference_Architecture%3A_Service_Discovery_and_Load_Balancing_with_Docker_Universal_Control_Plane_(UCP)).

For example, in this instance, the closing single quote for the value should be placed at the end, not after listenAddr:

    <^>value='listenAddr'<^^> = ":8080" dockerURL = "tcp://1.2.3.4.example.com:2376" \
      {"action":"set","node":
      {"key":"/interlock/v1/config","value":"listenAddr","modifiedIndex":194110,"createdIndex":194110},"prevNode":
      {"key":"/interlock/v1/config","value":"","modifiedIndex":194099,"createdIndex":194099}}

The correct syntax would be:

    <^>value='listenAddr<^^> = ":8080" dockerURL = "tcp://slx‐dockermaster01.uncg.edu:2376" \
      {"action":"set","node":
      {"key":"/interlock/v1/config","value":"listenAddr","modifiedIndex":194110,"createdIndex":194110},"prevNode":
      {"key":"/interlock/v1/config","value":"","modifiedIndex":194099,"createdIndex":194099}}
    '

### Resolution

Fix the syntax, and rerun the `curl` command. If further errors or different errors persist review the syntax again.
