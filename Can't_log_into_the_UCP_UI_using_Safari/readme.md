---
type: kbase
version: 2
title: "Can't log into the UCP UI using Safari"
summary: "The problem is that none of the certificates in your Keychain are trusted by UCP, so when you select a certificate, UCP won't be able to validate it, and won't be able to establish a TLS connection. If you already chose a certificate, Safari will save your preferences and automatically authenticate with the certificate you chose, so you need to clean your preferences and make Safari prompt you again for a choice:"
dateCreated: "Thu, 29 Jun 2017 22:46:12 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 0
internal: no
author: "tfoxnc"
platform:
testedon:
tags:
  - certs
product:
  - EE
uniqueid: KB000192
---

TESTING TESTING TESTING AND MORE TESTING!!!

When trying to log into the UCP web UI using Safari, you might be prompted for a client certificate.

This happens because UCP uses mutual TLS to ensure your data is kept safe. Most browsers, including Chrome and Firefox validate the TLS certificate presented by UCP, but don't send a client certificate to UCP, since that is optional.

Safari behaves slightly differently and always tries to send a client certificate to UCP, so you're prompted to choose one. The problem is that none of the certificates in your Keychain are trusted by UCP, so when you select a certificate, UCP won't be able to validate it, and won't be able to establish a TLS connection.

To solve this, you just need to tell Safari to not use any client certificate as shown here:

![](./images/27058244-ec2e0e24-4f84-11e7-8947-96b631e0401b.gif)

If you already chose a certificate, Safari will save your preferences and automatically authenticate with the certificate you chose, so you need to clean your preferences and make Safari prompt you again for a choice:

* Open **Keychain Access** from **Applications** > **Utilities**
* Select **Login Keychain and All Items**
* Search for `identity preference`
* Find the entry corresponding to the UCP IP address or domain name, and delete it.

When you try logging into UCP, you'll be prompted for a choice.
