---
type: kbase
version: 4
title: "How can I customize VPC/IAM elements in Docker Cloud using the dashboard?"
summary: "How can I customize VPC/IAM elements in Docker Cloud using the dashboard? As part of the deprecation of Classic platform, users have now the ability to choose any of the elements explained above through the API or the dashboard. Auto - Delegates to Docker Cloud the creation of the VPC. Auto - Delegates the management of the subnets to Docker Cloud. my_instance_role_name - You can select one of the IAM roles already created by you."
dateCreated: "Thu, 08 Oct 2015 19:26:55 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 1
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

How can I customize VPC/IAM elements in Docker Cloud using the dashboard?

## Answer

As part of the deprecation of Classic platform, users have now the ability to choose any of the elements explained above through the API or the dashboard.

In the launch node cluster view, you can choose:

* VPC dropdown: 
    1. `Auto` - Delegates to Docker Cloud the creation of the VPC.
    2. `vpc-XXXX (dc-vpc)` - Docker Cloud’s default VPC. It will be present if you have already deployed nodes to that region. Note that you can choose subnets and security groups with it. See “Which objects does Docker Cloud create in my EC2 account” for detailed info.
    3. `vpc-XXXX` - You can select one of the VPCs already created by you. If you tag name them, it will be displayed too.
* Subnets dropdown: 
    1. `Auto` - Delegates the management of the subnets to Docker Cloud. Will create them if they do not exist or will use the ones tagged with `dc-subnet`.
    2. Multiple selection of existing subnets. “See How does Docker Cloud balance my nodes among different availability zones?” section for detailed info.
* Security groups dropdown: 
    1. `Auto`
    2. Multiple selection of existing security groups.
* IAM roles dropdown: 
    1. `None` - Docker Cloud does not apply any instance profiles to the node.
    2. `my_instance_role_name` - You can select one of the IAM roles already created by you.
