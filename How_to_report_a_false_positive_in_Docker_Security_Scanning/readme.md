---
type: kbase
version: 8
title: "How to report a false positive in Docker Security Scanning"
summary: "If you believe that Docker Security Scanning has incorrectly identified a harmful vulnerability in your image, categorized the severity in error, or has failed to detect a vulnerability, please do the following: If you are reporting an image that you think is incorrectly flagged as containing a harmful vulnerability, provide the link to the CVE identifier for your base operating system."
dateCreated: "Tue, 10 May 2016 21:47:11 GMT"
dateModified: "Sat, 15 Oct 2016 21:01:34 GMT"
dateModified_unix: 1476565294
upvote: 0
internal: no
author: "allenlai"
platform:
testedon:
tags:
  - security
product:
  - Hub
---

## Overview

If you believe that Docker Security Scanning has incorrectly identified a harmful vulnerability in your image, categorized the severity in error, or has failed to detect a vulnerability, please do the following:

1. Gather the following information: 
    * A link to the repository (e.g. namespace/reponame:tag)
    * Component name and version
    * Dockerfile
    * Screenshot of your scan
    * Reason(s) for your report. Please include as much information as possible.
    * If you are reporting an image that you think is incorrectly flagged as containing a harmful vulnerability, provide the link to the [CVE identifier](https://cve.mitre.org/about/faqs.html#b1 "https://cve.mitre.org/about/faqs.html#b1") for your base operating system. For example: [https://security-tracker.debian.org/.../CVE-2015-1854](https://security-tracker.debian.org/tracker/CVE-2015-1854 "https://security-tracker.debian.org/tracker/CVE-2015-1854").
2. Report the issue by [opening a support case](https://support.docker.com/ "https://support.docker.com/").

When you submit your report, you'll get an email receipt with additional details on the submission process. If the detection is determined to be invalid, we will adjust the scan results and provide details on when updated scan results will be available. Thank you for your submission.
