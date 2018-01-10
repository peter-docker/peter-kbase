---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to setup certificates for a DR site
internal: no             # set to yes to keep it internal-only
comment: "Inspired by SFDC 33072 and info provided by @programmerq"
type: kbase               # set to customerservice if applicable
author:  quaddo
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.4
platform:           # Optional. Keep all that apply.
  - linux
  - windows server
  - z systems
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - Docker for Azure
  - Docker for AWS
  - installing
  - security
---
## Issue

You are making plans to setup disaster recovery (DR) and want to have a primary site for your production cluster as well as a secondary site for your backup/failover cluste. You want to ensure that the SSL certificates you install are correctly setup to make the experience transparent to UCP/DTR users.

## Prerequisites

Before performing these steps, you must meet the following requirements:

- UCP version in both clusters must match one another
- DTR version in both clusters must match one another

## Resolution

In brief, the SSL certificate you choose to install needs to have the [Subject Alternative Name (SAN)](https://en.wikipedia.org/wiki/Subject_Alternative_Name) for each endpoint that the user connects to. The name for this is called a multiple domain SSL certificate, or simply a multi-domain certificate.

Keeping it simple, this means having at the very least the fully qualified domain names (FQDNs) for the following:

- UCP endpoint (load balanced or single node) for each cluster
- DTR endpoint (load balanced or single node) for each cluster

In practice (ie, this is not a strict requirement), it's also beneficial to include the FQDNs for any node which might require direct user access, either for production or testing purposes such as:

- UCP load balancer
- Each UCP node
- DTR load balancer
- Each DTR node

Given a scenario where each cluster has 3 UCP nodes and 3 DTR nodes, plus a load balancer for each cluster, you might need 14 SANs included in the multi-domain certificate.

Having these additional FQDNs allows for troubleshooting connectivity to individual nodes without requiring traffic to pass through a load balancer.

## What's Next

- [Subject Alternative Name](https://en.wikipedia.org/wiki/Subject_Alternative_Name)
- [What is a Multi-domain SSL certificate?](https://www.namecheap.com/support/knowledgebase/article.aspx/9280/2221/what-is-a-multidomain-ssl-certificate)
- [What is a Multi-Domain (SAN) Certificate?](https://www.digicert.com/multi-domain-ssl/)
- [Multi-Domain (SAN) Certificates - Using Subject Alternative Names](https://www.digicert.com/subject-alternative-name.htm)
- [Understanding Multi-Use Digital Certificates](https://wiki.apache.org/httpd/UnderstandingMultiUseSSLCertificates)
