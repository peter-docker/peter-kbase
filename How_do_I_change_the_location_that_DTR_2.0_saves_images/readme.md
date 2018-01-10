---
type: kbase
version: 5
title: "How do I change the location that DTR 2.0 saves images?"
summary: "To change the location that DTR 2.0 uses to save image, stop DTR and Docker Engine, change the mount point, and start DTR: Find the backing location of the volume. For example, if your instance ID is a9c967ae5ef2, the volume will be called dtr-registry-a9c967ae5ef2 and will be stored at /var/lib/docker/volumes/dtr-registry-a9c967ae5ef2/_data. You can verify the volume name and location using the following 2 commands: For example, to make /opt/bigdisk the backing store for the volume:"
dateCreated: "Wed, 01 Jun 2016 15:40:34 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "programmerq"
platform:
testedon:
  - dtr-2.0.0
tags:
product:
  - EE
---

To change the location that DTR 2.0 uses to save image, stop DTR and Docker Engine, change the mount point, and start DTR:

1. Stop DTR.
2. Find the backing location of the volume. For example, if your instance ID is `a9c967ae5ef2`, the volume will be called `dtr-registry-a9c967ae5ef2` and will be stored at `/var/lib/docker/volumes/dtr-registry-a9c967ae5ef2/_data`. You can verify the volume name and location using the following 2 commands: 
    
        docker volume ls
        docker volume inspect
3. Stop Docker Engine. Usually this can be done with one of the following commands, depending on your operating system: 
    
        sudo systemctl stop docker.service
        sudo service docker stop
4. Next, do a bind mount to this location. For example, to make `/opt/bigdisk` the backing store for the volume: 
    
        mount -o bind /opt/bigdisk /var/lib/docker/volumes/dtr-registry-a9c967ae5ef2/_data
5. Add this bind mount to your `/etc/fstab` so it will be in place on a reboot.
6. Restart Docker Engine. DTR will be started by up automatically with Engine: 
    
        sudo systemctl start docker.service
        sudo service docker start
7. The named volume should be backed by your new location. Verify with the following commands as before: 
    
        docker volume ls
        docker volume inspect