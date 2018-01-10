---
type: kbase
version: 3
title: "DTR 2.2.x installation appears successful, can't get to UI, dtr-nginx containers restarting"
summary: "WARN[0199] Couldn't confirm authentication works, but still completing installation: Failed to wait for dtr to come back up: Polling failed with 30 attempts 5s apart: error making request to openid/begin Get https://example.com/api/v0/openid/begin: read tcp x.x.x.x:42258->x.x.x.x:443: read: connection reset by peer If both of these conditions are met, you need to upgrade to seccomp 2.2.1, which is not installed with RHEL 7.2 by default."
dateCreated: "Wed, 05 Apr 2017 15:08:51 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
  - linux
testedon:
  - dtr-2.2.0
tags:
  - installing
  - error
product:
  - EE
---

When installing DTR 2.2.0 and higher on RHEL 7.2, the installation completes, but with the following warning:

    INFO[0189] Waiting for DTR to start... 
     
    INFO[0194] Waiting for DTR to start... 
    
    WARN[0199] Couldn't confirm authentication works, but still completing installation: Failed to wait for dtr to come back up: Polling failed with 30 attempts 5s apart: error making request to openid/begin Get https://example.com/api/v0/openid/begin: read tcp x.x.x.x:42258->x.x.x.x:443: read: connection reset by peer 
    
    INFO[0199] Installation is complete

If you receive this warning, check to see if the dtr-nginx container is continually restarting:

    {"level":"info","msg":"watching for changes to configtracker.configSpec{src:\"hub.yml\", writer:(configtracker.WriterFunc)(0x4e3030), templateFunc:(configtracker.TemplateFunc)(0x4e2700), cacheKey:\"5dec3f25-c638-45b4-a216-8b1e966d9a6d\"}","time":"2017-02-16T15:56:49Z"} 
    
    {"level":"fatal","msg":"failed to run nginx: signal: killed","time":"2017-02-16T15:57:04Z"} 
    
    *** /bin/nginxwrapper exited with status 1.

If both of these conditions are met, you need to upgrade to seccomp 2.2.1, which is not installed with RHEL 7.2 by default.

Download and manually install the newer version of libseccomp (2.2.1), install DTR, and then the dtr-nginx container will start successfully. Refer to [https://docs.docker.com/engine/secur...or-a-container](https://docs.docker.com/engine/security/seccomp/#passing-a-profile-for-a-container "https://docs.docker.com/engine/security/seccomp/#passing-a-profile-for-a-container") for details.
