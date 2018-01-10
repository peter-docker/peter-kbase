---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Requested access to the resource is denied when pushing an image via cli
internal: no             # set to yes to keep it internal-only
comment: "internal comments that will only show up in the GH source file"
type: kbase               # set to customerservice if applicable
author:  hannahagee
product:
  - D4M
  - Hub
testedon:
  - ce-17.06.2-ce-mac27
platform:           # Optional. Keep all that apply.
  - linux
  - mac
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - Docker Cloud
  - error
---
## Issue

Unable to push images to my Docker Hub repositories using Docker for Mac. Login looks successful, but pushing images fails.

- Error:
```
"Upload failed: denied: requested access to the resource is denied" 
"Attempting next endpoint for push after error: denied: requested access to the resource is denied" 
"Failed to check for presence of layer sha256:xxxx (sha256:xxxx) in docker.io/<user>/<repo>” error="errors:\ndenied: requested access to the resource is denied\nerror parsing HTTP 401 response body: unexpected end of JSON input: \"\"\n" 
"failed to mount layer sha256:xxxx (sha256:xxxx) from docker.io/<user>/<repo>: errors:\ndenied: requested access to the resource is denied\nunauthorized: authentication required\n" 
```

## Troubleshooting Steps

- Push updated images to currently used repositories
- Push image to newly created repositories (able to create new repositories but can't push to them) 
- Try to push manually (through docker push mylittleadventure/<image>:<tag>) 
- Try to log out/in (and with a few different syntaxes) 

Verified: 
- Able to log in successfully 
- Able to build successfully through automated builds 
- Billing plan seems to be active and correctly configured (with valid credit card, not expired) 
- Unable to push from any host whether it be server or personal machine and no matter the network 

## Resolution

There is a known issue that occurs with `docker login` via command line when using Docker for Mac. A `docker login` command shows a successful login, however Docker for Mac doesn't appear to honor the login. 

Please see the following GitHub issue for more context: 

[https://github.com/docker/for-mac/issues/2016](https://github.com/docker/for-mac/issues/2016 )

Does your password happen to include any special characters, specifically the ":" character? When the ":" character is included in a password, this issue will occur.
