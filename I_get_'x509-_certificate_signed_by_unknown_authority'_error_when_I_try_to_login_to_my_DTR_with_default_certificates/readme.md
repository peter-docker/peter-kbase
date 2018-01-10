---
type: kbase
version: 5
title: "I get 'x509: certificate signed by unknown authority' error when I try to login to my DTR with default certificates"
summary: "You can do so by running these commands on the nodes from where you want to access your DTR (be sure to replace <my-dtr-domain> with your DTR Domain name.): export DOMAIN_NAME=<my-dtr-domain> openssl s_client -connect $DOMAIN_NAME:443 -showcerts </dev/null 2>/dev/null | openssl x509 -outform PEM | tee /usr/local/share/ca-certificates/$DOMAIN_NAME.crt update-ca-certificates service docker restart"
dateCreated: "Sat, 09 Jul 2016 00:10:56 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 58
internal: no
author: "sabin.basyal"
platform:
  - linux
testedon:
tags:
  - certs
  - error
product:
  - EE
---

```
x509: certificate signed by unknown authority
```

This error message means that you do not have a trusted certificate. You need to trust the default certificates generated during your Docker Trusted Registry (DTR) installation.

You can do so by running these commands on the nodes from where you want to access your DTR (be sure to replace `<^><my-dtr-domain><^^>` with your DTR Domain name.):

#### CentOS/RHEL

    export DOMAIN_NAME=<^><my-dtr-domain><^^>
    export TCP_PORT=<^><dtr-port><^^>
    openssl s_client -connect $DOMAIN_NAME:$TCP_PORT -showcerts </dev/null 2>/dev/null | openssl x509 -outform PEM | tee /etc/pki/ca-trust/source/anchors/$DOMAIN_NAME.crt
    update-ca-trust
    /bin/systemctl restart docker.service

#### Ubuntu

    export DOMAIN_NAME=<^><my-dtr-domain><^^>
    export TCP_PORT=<^><dtr-port><^^>
    openssl s_client -connect $DOMAIN_NAME:$TCP_PORT -showcerts </dev/null 2>/dev/null | openssl x509 -outform PEM | tee /usr/local/share/ca-certificates/$DOMAIN_NAME.crt
    update-ca-certificates
    service docker restart
