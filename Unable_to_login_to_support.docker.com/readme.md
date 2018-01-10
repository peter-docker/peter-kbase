---
type: kbase
version: 2
title: "Unable to log into support.docker.com"
dateCreated: "Thu, 29 Jun 2017 16:35:28 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "hannahagee"
platform:
testedon:
tags:
product:
---

## **Issue**

Error when logging into [support.docker.com](http://support.docker.com) with Docker Hub/Cloud ID:

    Oops! It looks like there was an issue authenticating you to the Docker Support Site:
    REGISTRATION_HANDLER_ERROR: User is not set active or profile is not Docker Customer Community Plus.
    If you are a Docker EE subscriber please use the Customer Service form below to report this issue so that we can help resolve it.

## Resolution

Try to log in using this URL instead of [support.docker.com](http://support.docker.com):

[https://docker.force.com/DocCustomLoginPage](https://docker.force.com/DocCustomLoginPage)

Also try to log in using the following format:

```
[user@domain.com.docker](mailto:user@domain.com.docker "mailto:user@domain.com.docker")
```

Example:  

```
[bob.smith@gmail.com.docker](mailto:bob.smith@gmail.com.docker "mailto:bob.smith@gmail.com.docker")
```
