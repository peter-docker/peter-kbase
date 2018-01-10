---
type: kbase
version: 2
title: "How do I move repositories among organization in Docker Hub?"
summary: "I want to move repositories among organizations in Docker Hub Set the new tag on an image docker tag <old-organization>/<repo-name>:<tag-name> <new-organization>/<repo-name>:<tag-name> 3. Push the image on Docker Hub docker push <new-organization>/<repo-name>:<tag-name> 4. Remove the image from the host docker rmi <old-organization>/<repo-name>:<tag-name> And also go to the Docker Hub and click on Delete Repository."
dateCreated: "Fri, 04 Dec 2015 01:52:32 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 3
internal: no
author: "sabin.basyal"
platform:
testedon:
tags:
product:
---

## Overview

I want to move repositories among organizations in Docker Hub

## Goal

After completing this how-to you will have your repositories migrated to your desired Organization



If you want to move your repository among organizations, then you would need to do the following:  
1. Pull your image `docker pull <old-organization>/<repo-name>:<tag-name>`  
2. Set the new tag on an image  

`docker tag <old-organization>/<repo-name>:<tag-name> <new-organization>/<repo-name>:<tag-name>`  
3. Push the image on Docker Hub  

`docker push <new-organization>/<repo-name>:<tag-name>`  
4. Remove the image from the host  

`docker rmi <old-organization>/<repo-name>:<tag-name>`  
And also go to the Docker Hub and click on Delete Repository.

**Note**: In order to remove an image from the host, please make sure that there are no containers actively based on it.

##