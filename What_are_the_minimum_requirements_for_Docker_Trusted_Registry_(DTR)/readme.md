---
type: kbase
version: 2
title: "What are the minimum requirements for Docker Trusted Registry (DTR)?"
summary: "DISK (image storage): ​ The requirements for the storage of Docker container images used in conjunction with DTR will scale linearly with the size and number of images, as well as how much I/O will be conducted. Using an external storage mechanism will lessen the I/O requirements for the DTR server itself, but that external service will of course then need to be able to meet the I/O demands of the DTR installation."
dateCreated: "Tue, 09 Feb 2016 16:33:49 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 2
internal: no
author: "justinnevill"
platform:
testedon:
tags:
product:
  - EE
---

## Questions

* What are the minimum requirements for Docker Trusted Registry (DTR)?
* Why aren't the minimum requirements for DTR published?

## Answer

The minimum requirements for DTR aren't documented anywhere because those requirements are highly dependent on how much the installation is being used. DTR is made up of small binaries that don't do a whole lot other than serve up large files. The biggest bottlenecks will be introduced by how well your hardware can handle that task.

**RAM:** Generally 4GB is considered sufficient for smaller production installations.

**DISK (DTR itself, not image storage):** The current version of the DTR software itself takes up about 1.8GB. Generally it's preferable to have no less than 20-30GB of space available. This allows plenty of breathing room for upgrades, or running other software in Docker alongside DTR.

**DISK (image storage):** ​ The requirements for the storage of Docker container images used in conjunction with DTR will scale linearly with the size and number of images, as well as how much I/O will be conducted. There are several available storage backends to choose from:​ The local storage backend is the default, and stores docker images on your DTR server's local filesystem. You could introduce high performance drives or a SAN to meet the I/O demand of your installation with this driver. With the local filesystem driver, a significant amount of RAM is recommended in order to maximize the benefits of Linux' native storage caching. ​Other available storage drivers include S3, Azure, and Swift (among others). Using an external storage mechanism will lessen the I/O requirements for the DTR server itself, but that external service will of course then need to be able to meet the I/O demands of the DTR installation.

**CPU:** DTR is not very CPU intensive. Most modern dual core processors are likely to be sufficient.