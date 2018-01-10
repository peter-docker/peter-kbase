---
type: kbase
version: 2
title: "How do  I replace the TLS certificates for UCP?"
summary: "Docker Universal Control Plane uses TLS to encrypt the traffic between users and your cluster. If you installed UCP with the default self-signed certs, you can replace them with externally-signed certs after the installation process. To replace the server certificates used by UCP, for each controller node, in the directory where you have your keys and certificates, use the following commands:"
dateCreated: "Wed, 07 Sep 2016 18:57:26 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "tfoxnc"
platform:
testedon:
tags:
  - certs
product:
  - EE
---

Docker Universal Control Plane uses TLS to encrypt the traffic between users and your cluster. By default this is done using self-signed certificates.

If you installed UCP with the default self-signed certs, you can replace them with externally-signed certs after the installation process.

To replace the server certificates used by UCP, **for each controller node**, in the directory where you have your keys and certificates, use the following commands:

1. Create a container that attaches to the same volume where certificates are stored: 
    
        docker create --name replace-certs -v ucp-controller-server-certs:/data busybox
2. Copy your keys and certificates to the container's volumes: 
    
        docker cp cert.pem replace-certs:/data/cert.pem
        docker cp ca.pem replace-certs:/data/ca.pem
        docker cp key.pem replace-certs:/data/key.pem
3. Remove the container, since you won't need it any longer: 
    
        docker rm replace-certs
4. Restart the ucp-controller container: 
    
        docker restart ucp-controller
5. Communicate the change to users. Ask users to log into UCP and [download the updated certificate client bundle](https://docs.docker.com/ucp/access-ucp/cli-based-access/ "https://docs.docker.com/ucp/access-ucp/cli-based-access/").
