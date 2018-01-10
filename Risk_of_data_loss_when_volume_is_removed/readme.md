---
type: kbase
version: 5
title: "Risk of data loss when volume is removed"
summary: "If such a volume is removed with docker volume rm, the data in the backing store is lost. The bug can also be triggered if using Docker Trusted Registry (DTR) with NFS storage — if you installed DTR with the --nfs-storage-url option or if you manually mounted an NFS share in DTR. An update that fixes the bug will be made available for all Docker versions starting from 1.11 and later, and the bug will also be fixed in the upcoming Docker 17.06 release."
dateCreated: "Thu, 18 May 2017 20:42:03 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "tfoxnc"
platform:
testedon:
tags:
  - storage
product:
---

A bug was identified in the Docker platform that can cause loss of data on volumes when they are removed. The bug is triggered if a volume cannot be unmounted once it’s no longer used by any Docker containers, leaving the volume in an incorrect state. If such a volume is removed with `docker volume rm`, the data in the backing store is lost.

The bug will typically be triggered if using [local volumes with mount options](https://docs.docker.com/engine/reference/commandline/volume_create/#driver-specific-options) or using NFS-backed volumes. Volumes that use external drivers (volume plugins) are not affected.

The bug can also be triggered if using Docker Trusted Registry (DTR) with NFS storage — if you installed DTR with the `--nfs-storage-url` option or if you manually mounted an NFS share in DTR.

When DTR is using NFS `join`, `reconfigure`, `remove` or `destroy` , operations can result in DTR data loss.

An update that fixes the bug will be made available for all Docker versions starting from 1.11 and later, and the bug will also be fixed in the upcoming Docker 17.06 release.

## Impact

The bug affects all versions of Docker with volume support, including 1.11, 1.12, 1.13, 17.03 and earlier releases. Updates will be made available to all Docker versions starting from 1.11.

Docker Universal Control Plane and Docker Trusted Registry do not require separate updates, and you can continue to use your currently deployed version of UCP and DTR. Only the Docker EE platform (previously Docker CS Engine) itself requires updates to fix the bug.

If you are not using local or NFS volumes with Docker, no update is required. Docker encourages customers stay current and up-to-date with new Docker releases.

## Mitigation

Docker will make updates available to Docker 17.03, 1.13, 1.12 and 1.11.

Until updates from Docker are ready, the following temporary workarounds are available:

1. Before running `docker volume rm`, check the mount table with `mount -l` and manually unmount volumes with `umount /var/lib/docker/volumes/<name>/_data`.
2. If using DTR with NFS, avoid `join`, `reconfigure`, `remove` or `destroy` operations until an update is available.
3. For DTR, pre-create the volume as a Docker NFS volume instead of manually mounting an NFS share over the volume directory.

  
Please contact our [Support](https://support.docker.com "https://support.docker.com") if you have any questions or concerns.
