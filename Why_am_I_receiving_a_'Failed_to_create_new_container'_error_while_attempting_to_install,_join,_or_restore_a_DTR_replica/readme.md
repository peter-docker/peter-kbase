---
type: kbase
version: 4
title: "Why am I receiving a 'Failed to create new container' error while attempting to install, join, or restore a DTR replica?"
summary: "Failed to create new container dtr-nginx-12a34b56c78d: Couldn't create container 'dtr-nginx-12a34b56c78d' from image 'docker/dtr-nginx:2.0.4': Error response from daemon: Unable to find a node that satisfies the following conditions [port 80 (Bridge mode) port 443 (Bridge mode)] [available container slots] [container!=ucp-controller (soft=false) container!=dtr-api-* (soft=false)] [node==node1]"
dateCreated: "Fri, 18 Nov 2016 07:57:20 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "yiming"
platform:
testedon:
tags:
product:
  - EE
---

When trying to install, join, or restore a DTR replica, you might get the following error:

    Failed to create new container

There are multiple instances when you might receive this error. To debug this, check the constraints mentioned. They could look something like this:

    Failed to create new container dtr-nginx-12a34b56c78d: Couldn't create container 'dtr-nginx-12a34b56c78d' from image 'docker/dtr-nginx:2.0.4': Error response from daemon: Unable to find a node that satisfies the following conditions
    [port 80 (Bridge mode) port 443 (Bridge mode)]
    [available container slots]
    [container!=ucp-controller (soft=false) container!=dtr-api-* (soft=false)]
    [node==node1]

### Solution

The error could be caused by any of the constraints mentioned in the console output. Try the following:

1. Check that the node name mentioned in the constraints and ensure that it matches the name of the node listed in the UCP dashboard's **Node** page.
2. Ensure the ports mentioned are not currently in use by another process or container.
3. If you see the line 
    container!=dtr-api-*, check your UCP's **Admin Settings** page. Under the **Scheduler** settings, ensure the option "**Allow Administrators to deploy containers on UCP controllers or nodes running DTR**" is checked.
4. In the unlikely event that all three steps above are already verified and the issue still cannot be resolved, file a support ticket to further investigate the issue.