---
type: kbase
version: 9
title: "Why is my available data space less than expected?"
summary: "Sometimes, the available space reported is less than the total space minus the used space due to the loop device. Storage Driver: devicemapper ... Data Space Used: 1.482 GB Data Space Total: 107.4 GB Data Space Available: 3.84 GB ... You may want to consider adding additional space into the LVM pool used as the Device Mapper, as you may experience data integrity issues when you consume all of the physical space."
dateCreated: "Thu, 09 Jun 2016 20:35:19 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "pvnovarese"
platform:
  - linux
testedon:
tags:
  - daemon
  - storage
product:
  - EE
---

## Issue

The `docker info` command shows available data space. Sometimes, the available space reported is less than the total space minus the used space due to the loop device.

For example, in this output, you can see that the devicemapper storage driver is being used.
```
    Storage Driver: devicemapper
    ...
    Data Space Used: 1.482 GB
    Data Space Total: 107.4 GB
    Data Space Available: 3.84 GB
    ...
    
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    WARNING: Usage of loopback devices is strongly discouraged for production use. 
    Either use `--storage-opt dm.thinpooldev` or 
    use `--storage-opt dm.no_warn_on_loop_devices=true` to suppress this warning.
    ...
```
This loop device is effectively a sparse storage device. You can read about the details on the [docs.docker.com](https://docs.docker.com/engine/userguide/storagedriver/device-mapper-driver/).

### Resolution

The "available" number reflects how much space you physically have remaining. Additionally, it is important to note that the loop device consumes physical space and will cause the numbers to not add up correctly. You may want to consider adding additional space into the LVM pool used as the Device Mapper, as you may experience data integrity issues when you consume all of the physical space.
