---
type: kbase
version: 3
title: "Which objects does Docker Cloud create in my EC2 account?"
summary: "Which objects does Docker Cloud create in my EC2 account? If you decide to let Docker Cloud create elements for you, it will create: A VPC with the tag name dc-vpc and CIDR range 10.78.0.0/16. A set of subnets if there are no subnets already created in the VPC. Docker Cloud creates a subnet in every Availability Zone (AZ) possible, and leaves enough CIDR space for the user to create customized subnets. A route table name dc-route-table in the VPC, associating the subnet with the gateway."
dateCreated: "Thu, 08 Oct 2015 19:25:43 GMT"
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

Which objects does Docker Cloud create in my EC2 account?

## Answer

If you decide to let Docker Cloud create elements for you, it will create:

* A VPC with the tag name `dc-vpc` and CIDR range `10.78.0.0/16`.
* A set of subnets if there are no subnets already created in the VPC. Docker Cloud creates a subnet in every Availability Zone (AZ) possible, and leaves enough CIDR space for the user to create customized subnets. Every subnet created is tagged with`dc-subnet`.
* An internet gateway named `dc-gateway` attached to the VPC.
* A route table name `dc-route-table` in the VPC, associating the subnet with the gateway.
