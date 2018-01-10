---
type: kbase
version: 4
title: "Images missing in DTR 1.4"
summary: "After upgrading to DTR 1.4, the images are not visible in the web GUI, and trying to do a docker push results in a 500 error. Attempt to push an image while logged in with a valid user to see if you get the 500 error: docker login dtr.example.com; docker push dtr.example.com/bob/busybox If the output is blank, then your images need to be moved. On the DTR host, run du -sch /var/local/dtr/image-storage/{local,storage}. If storage is empty, then your images will need to be moved."
dateCreated: "Wed, 09 Dec 2015 19:00:05 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "programmerq"
platform:
testedon:
  - dtr-1.4.0
tags:
  - error
product:
  - EE
---

### Overview

After upgrading to DTR 1.4, the images are not visible in the web GUI, and trying to do a `docker push` results in a 500 error.

Error Message

Repositories created in the DTR GUI show up, but their corresponding tags don't.

Pushes fail with a 500 error:

    $ docker push  dtr.example.com/bob/busybox:latest
    
    The push refers to a repository [dtr.example.com/bob/busybox] (len: 1)
    
    ac6a7980c6c2: Pushed
    
    c00ef186408b: Pushed
    
    latest: digest: sha256:f3b487ea1fcaaf7c9409d8005e66a70f71d98f913dc6b8b4b07592e69938ad16 size: 2742

### Diagnostic Steps

1. Attempt to push an image while logged in with a valid user to see if you get the 500 error:  
    docker login <^>dtr.example.com<^^>; docker push <^>dtr.example.com<^^>/bob/busybox
2. Use ssh to connect to the DTR host. Look at `/usr/local/etc/dtr/storage.yml`. With DTR 1.4, it should have `rootdirectory: /storage`. If it has `rootdirectory: /local`, then it needs to be updated.
3. Execute `docker exec -it docker_trusted_registry_image_storage_0 find /storage`. If the output is blank, then your images need to be moved. On the DTR host, run `du -sch /var/local/dtr/image-storage/{local,storage}`. If storage is empty, then your images will need to be moved.

### Resolution

1. Stop DTR: 
    
        sudo docker run --rm docker/trusted-registry stop | sudo bash
2. Update your `/usr/local/etc/dtr/storage.yml` file to set `rootdirectory: /storage`
3. Move your existing files in `/var/local/dtr/image-storage/` to the correct location: 
    
        mv /var/local/dtr/image-storage/storage /var/local/dtr/image-storage/storage-bak && mv /var/local/dtr/image-storage/local /var/local/dtr/image-storage/storage
4. Start DTR: 
    
        sudo docker run --rm docker/trusted-registry start | sudo bash
5. Verify DTR shows your old tags in the web UI.
6. Verify you can push without getting a 500 error.
