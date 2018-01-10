---
type: kbase
version: 4
title: "How to update DTR certs after they've expired and UI is inaccessible"
summary: "When certs have expired they may be updated in UCP, but when attempting to update in DTR the UI is inaccessible. After completing these steps, you will have the ability to update the expired certs in DTR. Check all 3 replicas to verify that all DTR containers are running and not 'restarting': This will cause DTR to re-generate the self-signed certs. Run the reconfigure command again using the ACTUAL DTR URL with the --dtr-external-url flag. Log into DTR and update the certs from within the UI."
dateCreated: "Tue, 26 Sep 2017 14:42:28 GMT"
dateModified: "Wed, 04 Oct 2017 03:52:51 GMT"
dateModified_unix: 1507089171
upvote: 0
internal: no
author: "scasey"
platform:
  - linux
testedon:
tags:
product:
  - EE
---

## Issue

When certs have expired they may be updated in UCP, but when attempting to update in DTR the UI is inaccessible.

## Resolution

After completing these steps, you will have the ability to update the expired certs in DTR.

1. Check all 3 replicas to verify that all DTR containers are running and not "restarting": 
    
        $ sudo docker ps -a | grep dtr
2. Run dtr/reconfigure command passing the `--dtr-external-url` flag, providing a WRONG `dtr-external-url`. This will cause DTR to re-generate the self-signed certs. [https://docs.docker.com/datacenter/dtr/2.3/reference/cli/reconfigure/#options](https://docs.docker.com/datacenter/dtr/2.3/reference/cli/reconfigure/#options "https://docs.docker.com/datacenter/dtr/2.3/reference/cli/reconfigure/#options")
3. Run the reconfigure command again using the ACTUAL DTR URL with the `--dtr-external-url` flag. This will allow you to log into DTR UI.
4. Log into DTR and update the certs from within the UI.