---
type: kbase
version: 3
title: "How can I customize VPC/IAM elements in Docker Cloud using the API?"
summary: "Question How can I customize VPC/IAM elements in Docker Cloud using the API? Answer Add the following section to your body parameters: 'provider_options' = { 'vpc': { # optional 'id': 'vpc-xxxxxxxx', # required 'subnets': ['subnet-xxxxxxxx', 'subnet-yyyyyyyy'], # optional 'security_groups': ['sg-xxxxxxxx'] # optional }, 'iam': { # optional 'instance_profile_name': 'my_instance_profile_name' # required } }"
dateCreated: "Thu, 08 Oct 2015 19:28:04 GMT"
dateModified: "Tue, 09 Aug 2016 13:47:16 GMT"
dateModified_unix: 1470750436
upvote: 0
internal: no
author: "justinnevill"
platform:
testedon:
tags:
  - API
  - aws
  - Docker Cloud
product:
  - Hub
---

## Question

How can I customize VPC/IAM elements in Docker Cloud using the API?

## Answer

Add the following section to your body parameters:

    "provider_options" = {
    
        "vpc": {                                                 # optional
    
            "id": "vpc-xxxxxxxx",                                # required
    
            "subnets": ["subnet-xxxxxxxx", "subnet-yyyyyyyy"],   # optional
    
            "security_groups": ["sg-xxxxxxxx"]                   # optional
    
        },
    
        "iam": {                                                 # optional
    
            "instance_profile_name": "my_instance_profile_name"  # required
    
        }
    
    }
