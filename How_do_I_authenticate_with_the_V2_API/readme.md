---
type: kbase
version: 2
title: "How do I authenticate with the V2 API?"
summary: "# get token to be able to talk to Docker Hub TOKEN=$(curl -s -H 'Content-Type: application/json' -X POST -d '{'username': ''${UNAME}'', 'password': ''${UPASS}''}' https://hub.docker.com/v2/users/login/ | jq -r .token) # build a list of all images & tags for i in ${REPO_LIST} do # get tags for repo IMAGE_TAGS=$(curl -s -H 'Authorization: JWT ${TOKEN}' https://hub.docker.com/v2/repositories/${UNAME}/${i}/tags/?page_size=10000 | jq -r '.results|.[]|.name')"
dateCreated: "Wed, 16 Mar 2016 18:42:47 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 13
internal: no
author: "kizbitz"
platform:
testedon:
tags:
product:
  - Hub
---

The following example script demonstrates authentication with the new V2 API.

Notes:

* `jq` required
* Need to set user/pass

```
#!/bin/bash

set -e

# set username and password
UNAME="<^>username<^^>"
UPASS="<^>password<^^>"

# get token to be able to talk to Docker Hub
TOKEN=$(curl -s -H "Content-Type: application/json" -X POST -d '{"username": "'${UNAME}'", "password": "'${UPASS}'"}' https://hub.docker.com/v2/users/login/ | jq -r .token)

# get list of namespaces accessible by user (not in use right now)
#NAMESPACES=$(curl -s -H "Authorization: JWT ${TOKEN}" https://hub.docker.com/v2/repositories/namespaces/ | jq -r '.namespaces|.[]')

# get list of repos for that user account
REPO_LIST=$(curl -s -H "Authorization: JWT ${TOKEN}" https://hub.docker.com/v2/repositories/${UNAME}/?page_size=10000 | jq -r '.results|.[]|.name')

# build a list of all images & tags
for i in ${REPO_LIST}
do
  # get tags for repo
  IMAGE_TAGS=$(curl -s -H "Authorization: JWT ${TOKEN}" https://hub.docker.com/v2/repositories/${UNAME}/${i}/tags/?page_size=10000 | jq -r '.results|.[]|.name')

  # build a list of images from tags
  for j in ${IMAGE_TAGS}
  do
    # add each tag to list
    FULL_IMAGE_LIST="${FULL_IMAGE_LIST} ${UNAME}/${i}:${j}"
  done
done

# output list of all docker images
for i in ${FULL_IMAGE_LIST}
do
  echo ${i}
done
```
