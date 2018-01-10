---
type: kbase
version: 2
title: "My AWS account does not link. How do I troubleshoot it?"
summary: "My AWS accound does not link. How do I troubleshoot it? To validate your AWS Security Credentials, Docker Cloud tries to dry-run an instance on every region. Credentials will be valid if the operation succeeds at least in one of the regions. If you get the following message Invalid AWS credentials or insufficient EC2 permissions follow these steps to troubleshoot it: You can look for the AMI in the region you want to deploy to here."
dateCreated: "Thu, 08 Oct 2015 19:22:11 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 14
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

My AWS accound does not link. How do I troubleshoot it?

## Answer

To validate your AWS Security Credentials, Docker Cloud tries to dry-run an instance on every region. Credentials will be valid if the operation succeeds at least in one of the regions. If you get the following message `Invalid AWS credentials or insufficient EC2 permissions` follow these steps to troubleshoot it:

1. [Download AWS CLI](https://aws.amazon.com/cli/) and [configure it](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) with your Security Credentials.

2. Run the following command:
    
        aws ec2 run-instances --dry-run --image-id ami-4d883350 --instance-type m3.medium

This will try to dry-run an Ubuntu 14.04 LTS 64-bit in `sa-east-1` (Sao Paulo, South America). You can look for the AMI in the region you want to deploy to [here](http://cloud-images.ubuntu.com/locator/ec2/). It should show you the error message. If everything is ok, you will see the following message:

    A client error (DryRunOperation) occurred when calling the RunInstances operation: Request would have succeeded, but DryRun flag is set.
