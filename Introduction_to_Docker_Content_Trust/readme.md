---
type: guide
version: 5
title: "Introduction to Docker Content Trust"
summary: "When a publisher using Docker Content Trust pushes an image to a remote registry, Docker Engine signs the image locally with the publisher’s private key. When a user later pulls this image, Docker Engine uses the publisher’s public key to verify that the image is exactly what the publisher created, has not been tampered with, and is up to date."
dateCreated: "Thu, 15 Oct 2015 19:03:59 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "pvnovarese"
platform:
  - linux
testedon:
  - cse-1.9.0-cs1
tags:
  - Docker Content Trust
product:
  - EE
---

This guide provides an overview of Docker Content Trust and some quick familiarization exercises. It was introduced in Docker Engine 1.8 and Docker CS Engine 1.9.0 an is available in Docker EE.

Docker Content Trust 
(DCT) provides strong cryptographic guarantees over what code and what versions of software are being run in your infrastructure. Docker Content Trust integrates
[The Update Framework (TUF)](http://theupdateframework.com/)
into Docker using
[Notary](https://youtu.be/at72dhg-SZY?t=4873)
, an open source tool that provides trust over any content.

When a publisher using Docker Content Trust pushes an image to a remote registry, Docker Engine signs the image locally with the publisher’s private key. When a user later pulls this image, Docker Engine uses the publisher’s public key to verify that the image is exactly what the publisher created, has not been tampered with, and is up to date.

More details about the internals of DCT can be found in the [Docker Blog.](https://blog.docker.com/2015/08/content-trust-docker-1-8/ "https://blog.docker.com/2015/08/content-trust-docker-1-8/")

More information about using DCT can be found on [doc.docker.com](https://docs.docker.com/engine/security/trust/content_trust/ "https://docs.docker.com/engine/security/trust/content_trust/").

### DCT Motivations

* Image Provenance is critical for Production
* Distributed content should be signed
* Key compromise should be difficult
* Compromise resilience is important
* Replay attacks should be hard
* Existing infrastructure is reused
* Trusting Docker should not be mandatory

### Pull Images with Docker Content Trust

    docker pull --disable-content-trust jpetazzo/clock

    docker -D pull --disable-content-trust hello-world
    docker -D pull hello-world

* Temporarily disable content trust to successfully pull unsigned content:
* Enable debug mode to compare the behavior of pulls where signed content is verified to pulls where no trust data is verified:
* Enable DCT: 
    
        export DOCKER_CONTENT_TRUST=1
* Try to pull unsigned content and observe error messages: 
    
        docker pull jpetazzo/clock

### Sign and Push Images with DCT

* Log into Docker Hub with Docker Engine 1.8, Docker CS Engine 1.9.0, or newer: 
    
        docker login
* With DCT enabled, push an image to Hub. When you push, Docker will note you have no keys, create them, and prompt you for a passphrase to encrypt them: 
    
        docker tag <^><clock image ID><^^> <^><HubUsername><^^>/clock:latest
        docker -D push <^><username><^^>/clock:latest 
        Enter key passphrase for offline key with id <^><yourIDnumber><^^>: 
        Enter passphrase for new tagging key with id docker.io/<^><HubUsername><^^>/clock (<^><yourIDnumber><^^>):
