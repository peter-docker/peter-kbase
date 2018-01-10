---
type: kbase
version: 4
title: "Why do I get intermittent failures when I try to push to repositories on Docker Hub?"
summary: "Question Why do I get intermittent failures when I try and upload to repositories on Docker Hub? Answer This can sometimes occur due to DNS resolution problems on the client machine. Try changing the default DNS servers in /etc/resolv.conf to use Google's public DNS resolvers: nameserver 8.8.8.8 nameserver 8.8.4.4 If this resolves the issue then additional investigation of the original DNS servers may be needed."
dateCreated: "Tue, 12 Apr 2016 22:00:04 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 1
internal: no
author: "nhazlett"
platform:
  - linux
testedon:
tags:
product:
  - Hub
---

## Question

Why do I get intermittent failures when I try and upload to repositories on Docker Hub?

## Answer

This can sometimes occur due to DNS resolution problems on the client machine. Try changing the default DNS servers in `/etc/resolv.conf` to use Google's public DNS resolvers:

    nameserver 8.8.8.8
    nameserver 8.8.4.4

If this resolves the issue then additional investigation of the original DNS servers may be needed.
