---
type: kbase
version: 2
title: "What is the difference between EC2-Classic and EC2-VPC?"
summary: "To add a compatibility layer between Classic and VPC, Amazon detects if your account only supports VPC platform, and when you deploy an instance in Classic mode it does not fail, but places the instance in the default VPC (with a default subnet) created in the region for you. This way you can now define and customize your own VPC in AWS portal, name it as dc-vpc and deploy a node cluster in the region the VPC was created."
dateCreated: "Thu, 08 Oct 2015 19:23:50 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 7
internal: no
author: "justinnevill"
platform:
testedon:
tags:
  - aws
  - Docker Cloud
product:
  - Hub
---

## Question

What is the difference between EC2-Classic & EC2-VPC?

## Answer

According to [Amazon EC2 documentation](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) there are two supported platforms to deploy your instances:

| Platform | Introduced in | Description |
| --- | --- | --- |
| EC2-Classic | The original release of Amazon EC2   | Your instances run in a single, flat network that you share with other customers. |
| EC2-VPC | The original release of Amazon VPC | Your instances run in a virtual private cloud (VPC) that's logically isolated to your AWS account. |

Quoting the same documentation:

    Your account may support both the EC2-VPC and EC2-Classic platforms, on a region-by-region basis. If you created your account after 2013-12-04, it supports EC2-VPC only. To find out which platforms your account supports, see Supported Platforms. If your accounts supports EC2-VPC only, we create a default VPC for you. A default VPC is a VPC that is already configured and ready for you to use. [...]

Docker Cloud strives to maintain compatibility with both platforms, so the steps to deploy a managed instance launched by Docker Cloud are:

1. Create an SSH keypair named `dockercloud-<uuid>`.
2. Try to deploy the instance in Classic platform. If you see any errors (i.e. type of instance cannot be located without a VPC, like `t2` instances), then try to deploy the instance in VPC platform.
3. In VPC platform, look for a VPC tagged with the name `dc-vpc`. If one exists, use it to deploy the instance. If one does not exist, create it, and create any elements needed for the deployment (subnets, security groups, route tables, gateways).
4. Create an EBS volume of type `gp2` with the size specified on the **Create node cluster wizard** or with the parameter `disk` through the [API](https://docs.docker.com/apidocs/docker-cloud/#create-a-new-node-cluster). This disk will be used to store Docker containers and images.

To add a compatibility layer between Classic and VPC, Amazon detects if your account only supports VPC platform, and when you deploy an instance in Classic mode it does not fail, but places the instance in the default VPC (with a default subnet) created in the region for you. If your account only supports VPC, Docker Cloud deploys all your nodes in Amazon’s default VPC.

We believe that the VPC platform has greater benefits that Classic for the user, so we now only deploy to VPC. This removes step 1 in the Classic instructions. Instead, Docker Cloud always looks for a VPC tagged `dc-vpc`. This way you can now define and customize your own VPC in AWS portal, name it as `dc-vpc` and deploy a node cluster in the region the VPC was created. Docker Cloud will find it and use it! The rest of elements that Docker Cloud needs for the deployment are also customizable. You can check their tag names in the following section.
