---
type: guide
version: 8
title: "Troubleshooting External Certificates for UCP/DTR"
summary: "There are quite a few fields but you can leave some blank For some fields there will be a default value, If you enter '.', the field will be left blank. ----- Country Name (2 letter code) [AU]:US State or Province Name (full name) [Some-State]:Maryland Locality Name (eg, city) []:Annapolis Organization Name (eg, company) [Internet Widgits Pty Ltd]:Docker Solutions Organizational Unit Name (eg, section) []:Customer Success Common Name (e.g."
dateCreated: "Fri, 02 Sep 2016 20:10:03 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 1
internal: no
author: "clemenko"
platform:
testedon:
tags:
  - certs
product:
  - EE
---

## Goal

During installation, UCP/DTR creates certificates if not supplied. This guide assumes you are providing your own signed certificates. Specifically, this guide helps you

1. Understand what the files are used for and why
2. Troubleshoot certificate files for use with UCP/DTR

## Understanding Certificates for UCP

UCP has two sets of certificates — one for the front door (aka the external site) and another set for the internal cluster communication. This guide ONLY discusses the external set.

## Understanding the Files

Lets start with understanding what files are needed to request a signed certificate for UCP. Note these are not the same as the files that will be used later in configuring UCP external certificates. There are three files that you need:

1. `ca.pem` (CA) : This is the public certificate of the signing authority. Since the signing authority can be chained, you will need the intermediate certificates to be included into this file. If there are intermediates, then you should see at least two cert blocks.
2. `cert.crt` (CERT) : This is your public certificate received from the Certificate Authority.
3. `cert.key` (KEY) : This is your private certificate used to sign your Certificate Signing Request (CSR).

Why are there three files? Well, let's start by looking at the CERT and the KEY. This is YOUR key pair. If someone wanted to send you an encrypted message, they can use your Public Certificate to encrypt. Then you can decrypt it with your Private Key. How do you get a key pair?

Start by creating a key private key:

    openssl genrsa -out key.pem 2048

Now that you have a private key file named `key.pem` , use that KEY to create a Certificate Signing Request (CSR):

    openssl req -new -sha256 -key key.pem -out moby.csr

You will see output similar to the following. Provide the information at each prompt:

    You are about to be asked to enter information that will be incorporated into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank For some fields there will be a default value, If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:<^>US<^^>
    State or Province Name (full name) [Some-State]:<^>Maryland<^^>
    Locality Name (eg, city) []:<^>Annapolis<^^>
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:<^>Docker Solutions<^^>
    Organizational Unit Name (eg, section) []:<^>Customer Success<^^>
    Common Name (e.g. server FQDN or YOUR name) []:<^>example.com<^^>
    Email Address []:<^>mailto:me@example.com<^^>
    Please enter the following 'extra' attributes to be sent with your certificate request
    A challenge password []: An optional company name []:<^>Docker<^^>

Great. You now have two files, `key.pem` (KEY) and `moby.csr` (CSR). Next, send the CSR to your desired signing authority (i.e. Verisign, Digicert, or internal company signer). Once signed we will receive the public cert file (CERT). We should also receive the certificate authorities public cert (CA). You will need this to prove who you are. Once done you should have 4 files : KEY, CA, CSR, and CERT.

## Troubleshooting

There are quite a few issues that can come up with certificates. You need to look for a few things:

1. Is your CERT signed by the CA?
2. Is it the correct CA for your company?
3. Has your CERT expired?
4. Do you have all the intermediate CAs in the CA file?

Here are some steps on how to check/verify the files.

First, look at the CA to ensure that it is valid. Use the following command to look at the text of the certificate:

    openssl x509 -noout -text -in cert.pem | less

IF both the `ca.pem` (CA) and the `cert.pem` (CERT) check out, you can they verify that the CA signed the CERT:

    openssl verify -verbose -CAfile ca.pem cert.pem

This should give a VERY good idea if the files are good and that the CERT is signed by the CA.

## Remote Troubleshooting

If you need to troubleshoot a running system there are some openSSL commands that will let you do this.

To check the CERT of a specific domain:

    echo "" | openssl s_client -connect <^>example.com<^^>:443 | openssl x509 -noout -text

To parse the output to check for Subject Alternative Names (SANs).

    echo "" | openssl s_client -connect <^>example.com<^^>:443  | openssl x509 -noout -text | grep "DNS\|IP"

The `openssl s_client` command has a few options that can help diagnosing SSL/TLS issues. You can verify the remote CERT with a local CA. This can help check for bad CAs loaded on the server.
