---
type: kbase
version: 2
title: "I can't reach Docker Hub from my home network"
summary: "If you are using the Docker Toolbox for Windows or Mac, you will need to restart it when switching between networks (such as between work & home network) in order to refresh the DNS service."
dateCreated: "Tue, 08 Dec 2015 11:51:48 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 1
internal: no
author: "fauzan.ariffin"
platform:
testedon:
tags:
product:
  - Hub
---

### Overview

Sometimes a timeout is experienced when trying to perform basic 
Dockercommands such as 
login, 
push, 
pull.

The example below shows a timeout error when trying to execute a 
docker push.

    $ docker push docker/hello-world     
    The push refers to a repository [docker.io/docker/hello-world] (len: 1)     
    Sending image list     
    Put https://index.docker.io/v1/repositories/docker/hello-world/:
     dial tcp: i/o timeout

### Diagnostic Steps

1. Please make sure that your machine is **connected to the Internet** and you are able to access [http://hub.docker.com](http://hub.docker.com "http://hub.docker.com").
2. Next, check your DNS configuration.  
      
    You can use the 
    nslookupcommand (for Mac OSX & Linux) to verify: 
    
        $ nslookup index.docker.io        
        Server:        127.0.1.1        
        Address:    127.0.1.1#53        
        Non-authoritative answer:        
        index.docker.io    canonical name = elb-io.us-east-1.aws.dckr.io.        
        elb-io.us-east-1.aws.dckr.io    canonical name = us-east-1-elbio-rm5bon1qaeo4-623296237.us-east-1.elb.amazonaws.com.        
        Name:    us-east-1-elbio-rm5bon1qaeo4-623296237.us-east-1.elb.amazonaws.com        
        Address: 52.22.246.223        
        Name:    us-east-1-elbio-rm5bon1qaeo4-623296237.us-east-1.elb.amazonaws.com        
        Address: 54.164.74.90        
        Name:    us-east-1-elbio-rm5bon1qaeo4-623296237.us-east-1.elb.amazonaws.com        
        Address: 52.22.95.15
    
    *Note: You may use reliable 3rd party DNS provider such as [Google DNS](https://developers.google.com/speed/public-dns/?hl=en "https://developers.google.com/speed/public-dns/?hl=en") or [OpenDNS](https://www.opendns.com/ "https://www.opendns.com/").*
3. If you are using the **Docker Toolbox for Windows or Mac**, you will need to **restart it** when switching between networks (such as between work & home network) in order to refresh the DNS service.