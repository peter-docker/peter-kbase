---
type: guide
version: 4
title: "Securing Docker Trusted Registry 1.x with TLS"
summary: "To be able to login to DTR via the Docker CLI to perform pull/push operations, you need to either i) Generate a self-signed certificate with a valid Domain Name/IP or ii) Obtain a CA-signed certificate for DTR’s Fully-Qualified Domain Name (FQDN). If you decide to use the self-generated certificate, you can skip to this step and continue to 'Trusting the Certificate' below."
dateCreated: "Tue, 09 Feb 2016 16:40:38 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 1
internal: no
author: "justinnevill"
platform:
  - linux
testedon:
  - dtr-1.4.0
tags:
product:
  - EE
---

> **Note:** This article only applies to DTR 1.x.

Docker Trusted Registry (DTR) 1.x uses HTTP over TLS for both UI and Docker CLI connections. In order to establish a secure TLS connection, three steps are required:

1. Obtaining the SSL Certificate and Key
2. Uploading the Certificate and Private Key to DTR
3. Trusting the Certificate

Let’s take a look at each step in detail.

## Obtaining the SSL Certificate and Key

When you install Docker Trusted Registry 1.x, a self-signed certificate and key will be generated for a blank domain name. The certificate will be used to establish a secure TLS connection via the UI. To be able to login to DTR via the Docker CLI to perform pull/push operations, you need to either i) Generate a self-signed certificate with a valid Domain Name/IP or ii) Obtain a CA-signed certificate for DTR’s Fully-Qualified Domain Name (FQDN).

To use the first option, you simply need to plug in a valid domain name (e.g dtr.enterprise.com) for DTR and restart it. A new certificate for that specific domain name will be generated and used. See Below:

![](./images/![](https://lh6.googleusercontent.com/)

If you would like to use your own CA-signed certificate, you need to ensure it’s in the right format. DTR expects a PEM-encoded certificate and key. DTR also expects the full certificate chain. For example, if your SSL certificate provider/issuer provided you the certificated with DER, P7B, PK7S..etc, you would need to first convert the full certificate to PEM (see [here](https://www.sslshopper.com/article-most-common-openssl-commands.html "https://www.sslshopper.com/article-most-common-openssl-commands.html") for more information). Then, you need to ensure that you have the full chain of certificates included. Typically, the certificate signer/issuer will provide you with the Server Certificate and Intermediate Certificate. The certificate chain is comprised of both of these certificates.

## Uploading the Certificate and Private Key to DTR

If you decide to use the self-generated certificate, you can skip to this step and continue to "Trusting the Certificate" below. If you are using a CA-signed certificate, by now you should have a PEM-encoded certificate chain for your DTR instance. You have two options to upload it to DTR, either through the UI or CLI.

* Using the UI: If you would like to use the UI, navigate to `Settings >> Security`. Then copy and paste your certificate chain and key. The certificate chain needs to be in the following format:

    -----BEGIN CERTIFICATE-----
    <<<< Server Certificate >>>>>
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
    <<<< Intermediate Certificate >>>>>
    -----END CERTIFICATE----

If your certificate chain also includes the root certificate. You’ll will need to add it after the Intermediate Certificate. You also need to plug in the RSA Private Key.

* Using the CLI: You need to create a file called server.pem on the host that DTR is running on. The path of this file is `/usr/local/etc/dtr/ssl/server.pem`. The file should have the following format:

    -----BEGIN CERTIFICATE-----
    <<<< Server Certificate >>>>>
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
    <<<< Intermediate Certificate >>>>>
    -----END CERTIFICATE-----
    -----BEGIN RSA PRIVATE KEY-----
    <<<< Private RSA Key >>>>>
    -----END RSA PRIVATE KEY-----

After you add the file to the `/usr/local/etc/dtr/ssl/` directory, you need to restart DTR. You can do so from CLI using:

    sudo bash -c "$(sudo docker run docker/trusted-registry restart)"

## Trusting the Certificate

You need to ensure that DTR certificates are installed on each client Docker daemon host. If the DTR certificate was signed by a CA that is already trusted on your hosts, then you should be able to skip this step.

The procedure for installing the Docker Trusted Registry certificates on each Linux distribution has slightly different steps. These steps can be referenced in the Docker Documentation at [https://docs.docker.com/docker-trusted-registry/configuration/#security](https://docs.docker.com/docker-trusted-registry/configuration/#security "https://docs.docker.com/docker-trusted-registry/configuration/#security").

If you have installed a CA-signed certificate on your DTR instance and are experiencing errors during login via the Docker CLI such as:

![](./images/![](https://lh6.googleusercontent.com/)

This could be due to “lack of trust” by not of having the DTR CA certificate present on the client Docker daemon host. Assuming you have correctly uploaded a valid certificate onto your DTR instance, a secure TLS connection can be established by validating your client against your DTR instance using two preferred options:

* Manually adding the DTR CA certificate via CLI. You will need to create both the `/certs.d` and `/<^><dtr-domain-name><^^>` directories to mimic this path `/etc/docker/certs.d/<^><dtr-domain-name><^^>`/ (e.g. <^>dtr.example.com<^^>). Next copy your `server.pem` certificate from the DTR instance to the new directory as follows:

    mkdir -p /etc/docker/certs.d/<^><dtr-domain-name><^^>
    cp server.pem /etc/docker/certs.d/<^><dtr-domain-name><^^>/ca.crt

* Running a Docker image that takes care of trusting the certs. This option involves running the following Docker image: [https://hub.docker.com/r/nicolaka/dtr-trust/](https://hub.docker.com/r/nicolaka/dtr-trust/ "https://hub.docker.com/r/nicolaka/dtr-trust/") in a container, which then automatically configures your client and establishes the trust needed for secure TLS as follows:

    export DTR_DOMAIN_NAME=<^><DOMAIN NAME or IP OF DTR><^^>
    docker run --rm -v /etc/docker/certs.d/:/etc/docker/certs.d/ -e DTR_DOMAIN_NAME=$DTR_DOMAIN_NAME nicolaka/dtr-trust

Note that using one of these two options will not trust the certificate across the entire machine, so other services can’t establish a TLS handshake with DTR, just Docker.

## Conclusion

Now you can log into your DTR instance from your Docker host and perform pull/push operations as follows:

    $ docker login $DTR_DOMAIN_NAME
    Username: 
    Password: 
    Email: 
    Login Succeeded
