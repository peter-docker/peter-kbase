---
type: kbase
version: 4
title: "When configuring the Docker EE repository for the first time, 'Error parsing config' occurs"
summary: "Loaded plugins: fastestmirror Repository 'docker-ee-stable-17.03': Error parsing config: Error parsing 'gpgkey = '$dockerurl/gpg'': URL must be http, ftp, file or https not '' Repository 'docker-ee-stable-17.03-debuginfo': Error parsing config: Error parsing 'gpgkey = '$dockerurl/gpg'': URL must be http, ftp, file or https not '' Repository 'docker-ee-stable-17.03-source': Error parsing config: Error parsing 'gpgkey = '$dockerurl/gpg'': URL must be http, ftp, file or https not '' Repository 'do…"
dateCreated: "Mon, 08 May 2017 18:33:11 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "squizzi"
platform:
  - linux
testedon:
tags:
product:
  - EE
---

When configuring the Docker EE `yum` repository for the first time, if the `$dockerurl` environment variable is not set, the following errors will occur when `yum makecache fast` is executed:

    Loaded plugins: fastestmirror
    Repository 'docker-ee-stable-17.03': Error parsing config: Error parsing "gpgkey = '$dockerurl/gpg'": URL must be http, ftp, file or https not ""
    Repository 'docker-ee-stable-17.03-debuginfo': Error parsing config: Error parsing "gpgkey = '$dockerurl/gpg'": URL must be http, ftp, file or https not ""
    Repository 'docker-ee-stable-17.03-source': Error parsing config: Error parsing "gpgkey = '$dockerurl/gpg'": URL must be http, ftp, file or https not ""
    Repository 'docker-ee-stable': Error parsing config: Error parsing "gpgkey = '$dockerurl/gpg'": URL must be http, ftp, file or https not ""
    Repository 'docker-ee-stable-debuginfo': Error parsing config: Error parsing "gpgkey = '$dockerurl/gpg'": URL must be http, ftp, file or https not ""
    Repository 'docker-ee-stable-source': Error parsing config: Error parsing "gpgkey = '$dockerurl/gpg'": URL must be http, ftp, file or https not ""
    Repository 'docker-ee-test': Error parsing config: Error parsing "gpgkey = '$dockerurl/gpg'": URL must be http, ftp, file or https not ""
    Repository 'docker-ee-test-debuginfo': Error parsing config: Error parsing "gpgkey = '$dockerurl/gpg'": URL must be http, ftp, file or https not ""
    Repository 'docker-ee-test-source': Error parsing config: Error parsing "gpgkey = '$dockerurl/gpg'": URL must be http, ftp, file or https not ""

> This issue is specific to environments running Red Hat Enterprise Linux and CentOS.

## Steps

This issue occurs when the `$dockerurl` variable has no value set or the wrong value set. The URL is blank or incorrect, and thus `yum` fails to parse it. To set this value and resolve the issue:

1. Go to [Install Docker](https://docs.docker.com/engine/installation/) and click on your distro (Red Hat Enterprise Linux or CentOS).
2. On the next page, follow the instructions to find the `DOCKER-EE-URL` value from the Docker Store.
3. `echo` the `DOCKER-EE-URL` value for your Linux distro into the `/etc/yum/vars/dockerurl` file with: 
    
        sudo echo '<DOCKER-EE-URL>' > /etc/yum/vars/dockerurl
4. Finally, re-run `yum makecache fast`.