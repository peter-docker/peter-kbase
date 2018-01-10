---
type: kbase
version: 4
title: "How do I test if DTR is correctly integrated with UCP?"
summary: "To determine whether or not your DTR is correctly integrated with your UCP, first make sure you have followed the steps in https://docs.docker.com/ucp/dtr-integration/ to integrate UCP with your DTR instance. They will show you how to push an image to your DTR and then pull that image from UCP If the deployment is successful, that means your UCP was able to pull images from your DTR without any problem."
dateCreated: "Mon, 02 May 2016 17:47:52 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "sabin.basyal"
platform:
testedon:
tags:
product:
  - EE
---

To determine whether or not your DTR is correctly integrated with your UCP, first make sure you have followed the steps in [https://docs.docker.com/ucp/dtr-integration/](https://docs.docker.com/ucp/dtr-integration/ "https://docs.docker.com/ucp/dtr-integration/") to integrate UCP with your DTR instance. This document also includes the steps to verify the integration.

The following steps will further help you ensure the integration. They will show you how to push an image to your DTR and then pull that image from UCP

NOTE: Replace `<^><your-dtr-instance><^^>` with the domain name for your DTR such as `<^>dtr.example.net<^^>` for you. Also make sure that this domain name can be reached from your UCP server before continuing.

1. From any machine, log into your DTR, pull an image ( such as `**busybox**`, from Docker Hub ), and then push it to DTR: 
    
        docker login
        docker pull busybox
        docker tag busybox <^><your-dtr-instance><^^>/admin/mybusybox
        docker login <^><your-dtr-instance><^^>
2. From your UCP, create a repository named `admin/mybusybox`.
3. Push the image to your DTR: 
    
        docker push <^><your-dtr-instance><^^>/admin/mybusybox
4. Go to the **Containers** page in your UCP, and click on **+Deploy Container**.
5. Deploy your container with the image you pushed, and click on **Run Container**.

If the deployment is successful, that means your UCP was able to pull images from your DTR without any problem.
