---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: What is the official Docker image scanning CVE library update release schedule?
internal: no 
comment: "Customer request in SF case # 00030386"
type: kbase
author: aathomas
product: 
  - EE
platform:
testedon: 
  - dtr-2.2.5
  - dtr-2.2.6
---

The following information pertains to the release schedule of Docker’s Image Scanner CVE Library. 

Docker’s core [CVE Database](https://www.docker.com/docker-cve-database) is specifically tied to Docker EE specific vulnerabilities. Docker's [documentation](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/configure/set-up-vulnerability-scans/#update-the-cve-scanning-database) includes the following information. When running the Image Scanner CVE Database using online mode, the default behavior for Docker Security Scanning is to check automatically for updates to the vulnerability database and to download them when available. DTR performs this checks for new CVE database updates at 3:00 AM UTC every day. If an update is found, it is downloaded and applied without interrupting any scans in progress. Once the update is complete, the security scanning system [looks for new vulnerabilities in the indexed components](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/configure/set-up-vulnerability-scans/#update-the-cve-scanning-database).

If your installation does not have access to the public Internet, you are able to use offline mode and upload the provided tar file provided in the Docker Store with [further details in the official documentation](https://dss-cve-updates.docker.com). To update the CVE database for your DTR instance when it cannot contact the update server, you [download and install a tar file](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/configure/set-up-vulnerability-scans/#update-the-cve-scanning-database) that contains the database updates.

Concerning the publishing rates of offline CVE tar databases provided by Docker, the security policy is as follows: *Docker supports [responsible disclosure](https://en.wikipedia.org/wiki/Responsible_disclosure) of vulnerabilities and ask, in the spirit of responsible disclosure, for sufficient time to patch the issue before [publishing the details](https://www.docker.com/docker-security), which is typically less than a one month publishing cycle.* 

Other security related resources include the direct URL your DTR uses to obtain [CVE databases](https://dss-cve-updates.docker.com) and [Docker’s main security page](https://www.docker.com/docker-security). It’s also recommended to subscribe to the [Docker Dev](https://groups.google.com/forum/#!forum/docker-dev) and [Docker User](https://groups.google.com/forum/#!forum/docker-user) forums concerning security announcements as well additional articles for [Docker’s main security page](https://www.docker.com/docker-security).

