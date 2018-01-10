---
type: kbase
version: 5
title: "How do I use an external TLS certificate for UCP?"
summary: "This explains how to use an external certificate with UCP 1.1: Then, after you install DTR following the DTR installation documentation, using that load balancer IP address to go to the WebUI. TLS Certificate: Certificate issued by a Certificate Authority. TLS private key: The key you used to generate your request for a TLS Certificate. TLS CA: The CA authority used to create your TLS certificate (root CA)."
dateCreated: "Fri, 22 Jul 2016 20:20:17 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 2
internal: no
author: "sabin.basyal"
platform:
testedon:
  - ucp-1.1.1
  - ucp-2.0.0
tags:
  - certs
product:
  - EE
---

This explains how to use an external certificate with UCP 1.1:

[https://docs.docker.com/datacenter/ucp/1.1/installation/plan-production-install/#using-external-cas](https://docs.docker.com/datacenter/ucp/1.1/installation/plan-production-install/#using-external-cas "https://docs.docker.com/datacenter/ucp/1.1/installation/plan-production-install/#using-external-cas")

or on UCP 2.0:

[https://docs.docker.com/datacenter/ucp/2.0/guides/installation/plan-production-install/#using-external-cas](https://docs.docker.com/datacenter/ucp/2.0/guides/installation/plan-production-install/#using-external-cas "https://docs.docker.com/datacenter/ucp/2.0/guides/installation/plan-production-install/#using-external-cas")

To summarize:

1. First, install DTR using `--dtr-external-url` to specify the load-balancer/Public Address for DTR.
2. Then, after you install DTR following [the DTR installation](https://docs.docker.com/datacenter/dtr/2.1/guides/install/ "https://docs.docker.com/datacenter/dtr/2.1/guides/install/") documentation, using that load balancer IP address to go to the WebUI.
3. Go to `Settings` -> `Domain` and change your Load Balancer/Public Address to the hostname ( e.g. `dtr.example.com`) and replace items in `Show TLS settings` with your certificates.

To describe what goes where:

1. TLS Certificate: Certificate issued by a Certificate Authority. If there are any intermediate certificates, they should be included here in the correct order. You can generate your own certificates for Trusted Registry using a public service or your enterprise's infrastructure.

2. TLS private key: The key you used to generate your request for a TLS Certificate.

3. TLS CA: The CA authority used to create your TLS certificate (root CA).
