---
type: kbase
version: 10
title: "Why do my TLS certificates not work with Docker Compose?"
summary: "For example, if my server certificate is for ucp.example.com, and it is signed by 'Acme Intermediary G3 CA' which is in turn signed by 'Acme Root G3 CA', which in turn is signed by itself, then I'd expect the following to be the case: Similarly, in the DTR graphical interface where you can upload certificates, the same holds true but for the TLS CA and TLS certificate fields, respectively."
dateCreated: "Wed, 17 Aug 2016 18:46:46 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "programmerq"
platform:
testedon:
tags:
  - certs
product:
  - EE
---

Different TLS clients are picky about different situations. Usually when the `docker-py` client (that `docker-compose` uses) has this issue, there's a certificate out of place.

Double check that the following are inside your `ucp-controller-server-certs` volume:

* `ca.pem` - This should have exactly one certificate, and that should be the root certificate authority for the whole chain.
* `cert.pem` - This should have the actual certificate for UCP, and **all** its intermediate certificates. The server certificate should be at the top. The next certificate should be the next certificate in the chain. The last certificate should be signed by the root certificate that appears in `ca.pem`.
* `key.pem*` - This should have only the private key file and nothing else.

For example, if my server certificate is for ucp.example.com, and it is signed by 'Acme Intermediary G3 CA' which is in turn signed by 'Acme Root G3 CA', which in turn is signed by itself, then I'd expect the following to be the case:

* `ca.pem` - This should have the 'Acme Root G3 CA' certificate only.
* `cert.pem` - This would have the 'ucp.example.com' certificate, followed by 'Acme Intermediary G3 CA'.
* `key.pem*` - This would have the private key for 'ucp.example.com'.

Similarly, in the DTR graphical interface where you can upload certificates, the same holds true but for the **TLS CA** and **TLS certificate** fields, respectively.

> **NOTE:**`key.pem` should never be shared with a third party, including Docker Support.
