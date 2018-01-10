---
type: kbase
version: 3
title: "When attempting to restore a UCP backup an invalid tar header error occurs"
summary: "When attempting to restore a UCP backup tar created with the backup command, the following invalid tar header error usually means that you are missing the passphrase: time='2017-05-08T15:33:14Z' level=info msg='Parsing backup file' time='2017-05-08T15:33:14Z' level=fatal msg='unable to parse backup file: [archive/tar:invalid tar header]' If the same passphrase is not provided during the restore procedure, the restore will be unable to parse the backup file."
dateCreated: "Mon, 15 May 2017 21:42:04 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "squizzi"
platform:
testedon:
tags:
 - backup
product:
  - EE
---

When attempting to restore a UCP backup `tar` created with the `backup` command, the following `invalid tar header` error usually means that you are missing the passphrase:

    time="2017-05-08T15:33:14Z" level=info msg="Parsing backup file"
    time="2017-05-08T15:33:14Z" level=fatal msg="unable to parse backup file: [archive/tar:invalid tar header]"

This issue can sometimes occur if a `backup` is encrypted with the optional passphrase flag for UCP. If the same passphrase is not provided during the restore procedure, the restore will be unable to parse the backup file.

To fix this error, ensure that you are using the `--passphrase` flag during the restore.

    docker run --rm -i --name ucp -v \
    /var/run/docker.sock:/var/run/docker.sock -v backup.tar:/config/backup.tar \
    docker/ucp:2.1.4 restore -i --passphrase 
    <your_passphrase>

> A feature request has been opened to help make the `fatal msg` that appears during this issue less ambiguous. This feature should arrive in a future release of UCP.
