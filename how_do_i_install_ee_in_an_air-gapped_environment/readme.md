---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How do I install Docker EE if I have an air-gapped environment?
internal: no             
comment: ""
type: kbase
author:  bryceryan-docker
product:       
  - ee         
testedon:  
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.4
platform:
  - linux
tags: 
  - installing
---
## Issue

I have an air-gapped environment, which is to say that my production systems are not connected to the open Internet. 
As a result, the Docker engine, UCP, and DTR installation instructions do not work because my systems cannot reach Docker Hub or Store
to retrieve those images. How can I install Docker Enterprise Edition (Docker EE) in an air-gapped environment?

## Resolution

Docker provides instructions for what we call an "offline installation." Essentially, it's necessary to download the images
onto a system that is connected to the Internet, disconnect that system from the Internet, transfer those files to the
air-gapped environment (typically through a controlled file copy operation), and load them onto the nodes where Docker EE
is being installed.

This process is documented in these links:
- [Offline engine install](https://docs.docker.com/engine/installation/linux/docker-ee/centos/#install-from-a-package) 
- [Offline UCP install](https://docs.docker.com/datacenter/ucp/2.2/guides/admin/install/install-offline/) 
- [Offline DTR install](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/install/install-offline/)

