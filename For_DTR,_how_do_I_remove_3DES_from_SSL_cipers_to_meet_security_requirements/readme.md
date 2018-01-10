---
type: kbase
version: 4
title: "For DTR, how do I remove 3DES from SSL cipers to meet security requirements?"
summary: "If not, follow these steps to create a new image with 3DES removed, backup DTR, uninstall DTR, and restore DTR using newly built image. You should implement a separate backup policy for the Docker images, taking in consideration whether your DTR installation is configured to store images on the file system or using a cloud provider. The DTR bootstrapper installs DTR and uses the images on the machine it's installing on."
dateCreated: "Tue, 28 Mar 2017 14:14:18 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
testedon:
  - dtr-2.2.3
tags:
product:
  - EE
---

This article explains how to remove SSL ciphers that use the 3DES encryption suite from Docker Trusted Registry prior to version 2.3. If your security standards require you to remove SSL ciphers that use 3DES encryption, consider upgrading to DTR 2.3 or higher instead of performing these steps.

## Prerequisites

Before performing these steps, you must meet the following requirements:

* Is there a security requirement to remove this support?

* Have you backed up DTR?

* Do you understand the risks of backup/restore and understand that it is not guaranteed?

* Can you upgrade to DTR 2.3 or higher instead?

## Steps

> These steps were written for DTR 2.2.3. If using another version, verify the tag used is appropriate for the environment.

**To remove 3DES from the ssl_ciphers option in dtr-nginx container, if possible upgrade to DTR 2.3.0 or higher. If not, follow these steps to create a new image with 3DES removed, backup DTR, uninstall DTR, and restore DTR using newly built image.**

1. Enter the dtr-nginx container:

    $ docker run --rm -it --name build --entrypoint sh docker/dtr-nginx:2.2.3

2. In the container, backup the NGINX file, and then edit the original file:

    # mv /bin/nginx /bin/nginx-orig
    # vi /bin/nginx

3. Modify the contents of the NGINX file to the following:

    #!/bin/sh 
    sed -i 's/DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!/DH+AES:RSA+AESGCM:RSA+AES:!/' /config/nginx.conf 
    exec /bin/nginx-orig "$@"

4. Save the file and exit it.

5. Make sure the file is executable:

    # chmod +x /bin/nginx

5. In another terminal outside the container:

    $docker commit -c 'ENTRYPOINT ["/init", "--skip-runit", "/bin/nginxwrapper"]' build dtr-nginx-hack

6. Stop the original container and rename the images:

    $ docker stop 
    
    $ docker tag docker/dtr-nginx:2.2.3 docker/dtr-nginx:2.2.3-orig 
    $ docker tag dtr-nginx-hack docker/dtr-nginx:2.2.3

7. Backup DTR after fully reading the documentation:

[DTR backups and recovery](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/backups-and-disaster-recovery/#backup-dtr-data "https://docs.docker.com/datacenter/dtr/2.2/guides/admin/backups-and-disaster-recovery/#backup-dtr-data")

The backup command does not create a backup of Docker images. You should implement a separate backup policy for the Docker images, taking in consideration whether your DTR installation is configured to store images on the file system or using a cloud provider. During restore, you need to separately restore the image contents.

8. Test the DTR backup:

[DTR backups and recovery](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/backups-and-disaster-recovery/#testing-backups "https://docs.docker.com/datacenter/dtr/2.2/guides/admin/backups-and-disaster-recovery/#testing-backups")

9. Uninstall DTR:

[Uninstall Docker Trusted Registry](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/install/uninstall/ "https://docs.docker.com/datacenter/dtr/2.2/guides/admin/install/uninstall/")

10. Restore DTR:

[Restore DTR data](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/backups-and-disaster-recovery/#restore-dtr-data "https://docs.docker.com/datacenter/dtr/2.2/guides/admin/backups-and-disaster-recovery/#restore-dtr-data")

* The DTR bootstrapper installs DTR and uses the images on the machine it's installing on.
* The `restore` command performs a fresh installation of DTR and reconfigures it with the configuration created during a backup. The command starts by installing DTR. Then it restores the configurations from the backup and then restores the repository metadata. Finally, it applies all of the configs specified as flags to the restore command.

* After restoring DTR, you must make sure that it’s configured to use the same storage backend where it can find the image data. If the image data was backed up separately, you must restore it now.

11. Verify the container running from the new image has 3DES removed:

    $ docker exec 3ae582ce08fa cat /config/nginx.conf | grep ssl_ciphers 
    ssl_ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:RSA+AESGCM:RSA+AES:!aNULL:!MD5:!DSS;