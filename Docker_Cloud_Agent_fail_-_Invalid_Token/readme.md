---
type: kbase
version: 2
title: "Docker Cloud Agent fail - Invalid Token"
summary: "If you are trying to register your own node to Docker Cloud but the registration fails with the error message Token is invalid in the log, you need to generate a new BYON token. When trying to register your own node to Docker Cloud, the following error message appears: This command will give you the command needed to bring your own node, including the token."
dateCreated: "Wed, 16 Mar 2016 09:31:24 GMT"
dateModified: "Fri, 11 Nov 2016 23:00:14 GMT"
dateModified_unix: 1478905214
upvote: 0
internal: no
author: "fauzan.ariffin"
platform:
testedon:
tags:
  - error
  - Docker Cloud
product:
  - Hub
---

### Issue

If you are trying to register your own node to Docker Cloud but the registration fails with the error message `Token is invalid` in the log, you need to generate a new BYON token.

### Symptoms

When trying to register your own node to Docker Cloud, the following error message appears:
```
2016/03/10 09:48:10 Registering in Docker Cloud via PATCH: https://cloud.docker.com/api/agent/v1/node/a9b962b9-eee4-4029-b523-6e5729946941
2016/03/10 09:48:11 PATCH error 401 :either UUID (a9b962b9-eee4-4029-b523-6e5729946941) or <^>Token is invalid<^^>
```
### Resolution

1. Obtain a new token by executing the following command: 
    
        docker-cloud node byo
2. This command will give you the command needed to bring your own node, including the token. 
```    
Docker Cloud lets you use your own servers as nodes to run containers. For this you have to install our agent.
  	Run the  following  command on your server:
        	
        
   	curl -Ls https://get.cloud.docker.com/ | sudo -H sh -s 63ad1c63ec5d431a9b31133e37e8a614
```    
