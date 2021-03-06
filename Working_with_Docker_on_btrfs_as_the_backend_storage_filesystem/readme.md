---
type: guide
version: 3
title: "Working with Docker on btrfs as the backend storage filesystem"
summary: "root@m1-host:-# df -lkh Filesystem Size Used Avail Use% Mounted on udev 3.9G 12K 3.9G 1% /dev tmpfs 799M 392K 799M 1% /run /dev/xvda1 7.8G 3.1G 4.3G 43% / none 4.0K 0 4.0K 0% /sys/fs/cgroup none 5.0M 0 5.0M 0% /run/lock none 3.9G 0 3.9G 0% /run/shm none 100M 0 100M 0% /run/user root@m1-host:-# mount /dev/xvdx /var/lib/docker root@m1-host:-# df -lkh Filesystem Size Used Avail Use% Mounted on udev 3.9G 12K 3.9G 1% /dev tmpfs 799M 392K 799M 1% /run /dev/xvda1 7.8G 3.1G 4.3G 43% / none 4.0K 0 4.0K …"
dateCreated: "Tue, 05 Jul 2016 00:54:47 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "anoop.kumar"
platform:
  - linux
testedon:
tags:
  - storage
product:
  - EE
---

### About btrfs and Docker

Btrfs (pronounced better-fs) is a highly advanced and fully stable Linux filesystem that offers many compelling and useful features like Copy-On-Write, snapshotting, Raiding, and thin provisioning. Btrfs is the default filesystem on some systems like SuSE / SLES and is being used by some very large financial organizations today. Within Docker, support for Btrfs is available in the form of a storage driver as of 1.10 (both open source and commercially supported versions). While SuSE / SLES include btrfs as the default filesystem, it can be installed on almost every other Linux flavor like Debian / Ubuntu and Red Hat / CentOS systems. With the problems we see in using devicemapper and questions as to devicemapper's maintainability given that OverlayFS seems to be gaining more focus and attention, btrfs seems like a very good replacement for devicemapper on Red Hat based systems as Docker's preferred storage driver.

### Working with btrfs

In the sections below, we shall see how easy it is to work with btrfs and specifically how to recover from disk out-of-space issues.

What we'll use here is a stock Ubuntu 14.04.3 LTS system. We'll install Docker 1.11, the btrfs package, configure Docker to use btrfs as the storage driver. We will then simulate a disk out-of-space issue and then look at steps to recover from it without losing any data or applications running on the system. For the purposes of understanding all commands are typed out by hand, and it is urged that these manual steps are automated to make the entire recovery process more reliable.

### Btrfs Quickstart

1. Let's login to our Ubuntu 14.04 box and check if btrfs is installed and available. Since, we will be dealing with making changes to filesystems it will be convenient to run all commands as root (or use sudo to prefix all commands).
    
        root@m1-host:-# btrfs
        The program 'btrfs' is currently not installed. You can install it by typing:
        apt-get install btrfs-tools
        root@m1-host:-#
    
    Expectedly, btrfs is not installed on this Ubuntu system

2. Let's install btrfs as shown in the output message above
    
        root@m1-host:-# apt-get install btrfs-tools
        Reading package lists... Done
        Building dependency tree
        Reading state information... Done
        The following extra packages will be installed:
          liblzo2-2
        The following NEW packages will be installed:
          btrfs-tools liblzo2-2
        0 upgraded, 2 newly installed, 0 to remove and 91 not upgraded.
        Need to get 380 kB of archives.
        After this operation, 2,692 kB of additional disk space will be used.
        Do you want to continue? [Y/n]
        Get:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu/ trusty-updates/main liblzo2-2 amd64 2.06-1.2ubuntu1.1 [46.1 kB]
        Get:2 http://us-east-1.ec2.archive.ubuntu.com/ubuntu/ trusty-updates/main btrfs-tools amd64 3.12-1ubuntu0.1 [334 kB]
        Fetched 380 kB in 0s (18.3 MB/s)
        Selecting previously unselected package liblzo2-2:amd64.
        (Reading database ... 62083 files and directories currently installed.)
        Preparing to unpack .../liblzo2-2_2.06-1.2ubuntu1.1_amd64.deb ...
        Unpacking liblzo2-2:amd64 (2.06-1.2ubuntu1.1) ...
        Selecting previously unselected package btrfs-tools.
        Preparing to unpack .../btrfs-tools_3.12-1ubuntu0.1_amd64.deb ...
        Unpacking btrfs-tools (3.12-1ubuntu0.1) ...
        Processing triggers for man-db (2.6.7.1-1ubuntu1) ...
        Setting up liblzo2-2:amd64 (2.06-1.2ubuntu1.1) ...
        Setting up btrfs-tools (3.12-1ubuntu0.1) ...
        update-initramfs: deferring update (trigger activated)
        Processing triggers for libc-bin (2.19-0ubuntu6.6) ...
        Processing triggers for initramfs-tools (0.103ubuntu4.2) ...
        update-initramfs: Generating /boot/initrd.img-3.13.0-87-generic
        root@m1-host:-#

3. Now the btrfs command should work:
    
        root@m1-host:-# btrfs --version
        Btrfs v3.12
        root@m1-host:-#

4. Also, ensure that the current version of docker is installed. The simplest way to get docker up and running is `curl -fsSL [https://get.docker.com](https://get.docker.com "https://get.docker.com") | sh`

5. Btrfs works with block devices, so we would need one. Let's see what all block devices are attached to this system, using the `lsblk` command:
    
        root@m1-host:-# lsblk
        NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
        xvda    202:0    0   8G  0 disk
        └─xvda1 202:1    0   8G  0 part /
        root@m1-host:-#
    
    There are no unpartitioned block devices on this system. The only one is the one on which the OS is installed. We need to add another one.
    
    Since this is an AWS instance, I'm attaching two EBS instances, each 4GB in size.
    
    Running `lsblk` command now should reflect the new block devices added.
    
        root@m1-host:-# lsblk
        NAME    MAJ:MIN  RM SIZE RO TYPE MOUNTPOINT
        xvda    202:0     0   8G  0 disk
        └─xvda1 202:1     0   8G  0 part /
        xvdx    202:5888  0   4G  0 disk
        xvdy    202:5888  0   4G  0 disk
        root@m1-host:-#

6. We need to change docker to use `btrfs` as the storage driver. Currently (as the default on Ubuntu), docker is using `extfs` as indicated by the output of `docker info | grep -i filesystem`
    
        root@m1-host:-# docker info | grep Filesystem
        WARNING: No swap limit support
         Backing Filesystem: extfs
        root@m1-host:-#
    
    So, let's shut down docker before we make any changes using `service docker stop`.
    
    *Note: This is assuming that this is a brand new docker installation and there are no images that need to be preserved. If there are, then those images would need to be either pushed to the hub / DTR or exported into tar files and backed up in some fashion.*

### Getting Docker to use Btrfs

1. Let's format the first disk `xvdx` to use the btrfs filesystem. The wonderful aspect of this exercise is that btrfs behaves as a logical volume manager and as such there is no need to partition the disk or anything. We could partition the disk if we really wanted to, and not because btrfs needed it. We'll use `mkfs.btrfs` for this task.
    
        root@m1-host:-# mkfs.btrfs -f /dev/xvdx
        
        WARNING! - Btrfs v3.12 IS EXPERIMENTAL
        WARNING! - see http://btrfs.wiki.kernel.org before using
        
        Turning ON incompat feature 'extref': increased hardlink limit per file to 65536
        fs created label (null) on /dev/xvdx
          nodesize 16384 leafsize 16384 sectorsize 4096 size 4.00GiB
        Btrfs v3.12
        root@m1-host:-#

2. Now that the btrfs filesystem has been created, let's mount it on the same path that docker uses as the root for all containers, images and volumes on the host which is `/var/lib/docker`.
    
        root@m1-host:-# df -lkh
        Filesystem      Size  Used Avail Use% Mounted on
        udev            3.9G   12K  3.9G   1% /dev
        tmpfs           799M  392K  799M   1% /run
        /dev/xvda1      7.8G  3.1G  4.3G  43% /
        none            4.0K     0  4.0K   0% /sys/fs/cgroup
        none            5.0M     0  5.0M   0% /run/lock
        none            3.9G     0  3.9G   0% /run/shm
        none            100M     0  100M   0% /run/user
        root@m1-host:-# mount /dev/xvdx /var/lib/docker
        root@m1-host:-# df -lkh
        Filesystem      Size  Used Avail Use% Mounted on
        udev            3.9G   12K  3.9G   1% /dev
        tmpfs           799M  392K  799M   1% /run
        /dev/xvda1      7.8G  3.1G  4.3G  43% /
        none            4.0K     0  4.0K   0% /sys/fs/cgroup
        none            5.0M     0  5.0M   0% /run/lock
        none            3.9G     0  3.9G   0% /run/shm
        none            100M     0  100M   0% /run/user
        /dev/xvdx       4.0G  320K  3.6G   1% /var/lib/docker
        root@m1-host:-#

3. Running `docker info` after bringing the docker daemon back up should reflect that docker is indeed using the `btrfs` (as the) storage driver.
    
        root@m1-host:-# service docker start
        docker start/running, process 30840
        root@m1-host:-# docker info | grep -i storage
        WARNING: No swap limit support
        Storage Driver: btrfs
        root@m1-host:-#

### Managing Disk Space Issues and Performance

1. Occasionally, we can expect to face 'out of disk space' issues, especially when dealing with big images or with images that have multiple tags and a CI / CD or automation system that could overwhelm the amount of available disk space. Handling disk space issues in Btrfs is actually quite trivial and it does not even require a restart of anything. Extending a volume with an additional device is as easy as running a single command on btrfs.
    
    In this section, we'll simulate conditions for an out of disk space condition and use btrfs to address it and see that docker continues to function with no hiccups

2. Let's simulate some very large Docker containers using the `fallocate` command: 
    
        root@m1-host:-# docker run -it --name big-fat-container ubuntu bash
        Unable to find image 'ubuntu:latest' locally
        latest: Pulling from library/ubuntu
        
        f069f1d21059: Pull complete
        ecbeec5633cf: Pull complete
        ea6f18256d63: Pull complete
        54bde7b02897: Pull complete
        Digest: sha256:bbfd93a02a8487edb60f20316ebc966ddc7aa123c2e609185450b96971020097
        Status: Downloaded newer image for ubuntu:latest
        root@234dbb1f6fba:/# fallocate -l 3.4G simulation.ftty
        root@234dbb1f6fba:/# root@m1-host:-#
    
    The output of `df -lkh` should reflect that the device has reached its storage threshold:
    
        root@m1-host:-# df -lkh /var/lib/docker
        Filesystem      Size  Used Avail Use% Mounted on
        /dev/xvdx       4.0G  3.6G   61M  99% /var/lib/docker
        root@m1-host:-#

3. Let's run another container, it would fail, expectedly as there is not enough disk space
    
        root@m1-host:-# docker run -d tomcat
        Unable to find image 'tomcat:latest' locally
        latest: Pulling from library/tomcat
        
        5c90d4a2d1a8: Downloading [==================================================>] 51.35 MB/51.35 MB
        ab30c63719b1: Download complete
        be275827e8b7: Download complete
        9aa4ff75c34e: Download complete
        a30607f3daa1: Downloading [>                                                  ] 1.058 MB/77.64 MB
        227937ba18b6: Download complete
        01a8aa3698c9: Download complete
        7e5d5c1983f4: Downloading [==================================================>] 3.016 MB/3.016 MB
        b9e603623f64: Waiting
        2618070f43c0: Waiting
        4012135ceed0: Waiting
        docker: write /var/lib/docker/tmp/GetImageBlob755659910: no space left on device.
        See 'docker run --help'.
        root@m1-host:-#

4. Btrfs' Logical Volume Management to the rescue. It is quite trivial now to easily expand our docker base directory (or btrfs volume, aptly) across additional, multiple disks. All it takes is a single command and an available block device to extend the volume upon. When we created`xvdx`, we also created an additional EBS device `xvdy`. We will now use this unformatted block device to extend the existing `/var/lib/docker` volume, as shown below. Once we do this, it is recommended to "balance" the data across the multiple disks to offer resilience and improve performance.
    
        root@m1-host:-# btrfs device add /dev/xvdy /var/lib/docker/
        root@m1-host:-# df -lkh /var/lib/docker
        Filesystem      Size  Used Avail Use% Mounted on
        /dev/xvdx       8.0G  3.6G  4.1G  47% /var/lib/docker
        root@m1-host:-#
    
    The output of `df -lkh` shows only 47% used as opposed to 99% used earlier when the volume was mounted on a single device. Matter of fact, when using Btrfs, the usual unix commands `df`etc would not show accurate information due to the way Btrfs uses RAID. Therefore, a more accurate representation of disk free space can be derived using `btrfs filesystem df /var/lib/docker`. The `filesystem` in the command may be shortened to `fi` as shown below:
    
        root@m1-host:-# btrfs fi df /var/lib/docker
        Data, single: total=3.57GiB, used=3.51GiB
        System, DUP: total=8.00MiB, used=16.00KiB
        System, single: total=4.00MiB, used=0.00
        Metadata, DUP: total=204.75MiB, used=10.95MiB
        Metadata, single: total=8.00MiB, used=0.00
        root@m1-host:-#
    
    Now, new images can be pulled and we can run containers without any disk space issues:
    
        root@m1-host:-# docker run -d tomcat
        Unable to find image 'tomcat:latest' locally
        latest: Pulling from library/tomcat
        
        5c90d4a2d1a8: Pull complete
        ab30c63719b1: Pull complete
        be275827e8b7: Pull complete
        9aa4ff75c34e: Pull complete
        a30607f3daa1: Pull complete
        227937ba18b6: Pull complete
        01a8aa3698c9: Pull complete
        7e5d5c1983f4: Pull complete
        b9e603623f64: Pull complete
        2618070f43c0: Pull complete
        4012135ceed0: Pull complete
        Digest: sha256:e7eb5e6620f88c94d403347033bd1698c73a2d9f91fca6d5dd4a4322bfde2379
        Status: Downloaded newer image for tomcat:latest
        33ee2f0dd564914510dd90c498b6ba559634d3329e63b81f6a03f9b6a6c4e338
        root@m1-host:-#

5. As mentioned earlier, for better performance and resilience, it is a recommended practice that the disks be balanced. We can see that the disks are not balanced as most of the data was written to the first disk only. This is shown using `btrfs fi show /var/lib/docker`
    
        root@m1-host:-# btrfs fi show /var/lib/docker
        Label: none  uuid: c6542f8f-66b6-45da-9552-87c21d4956a6
          Total devices 2 FS bytes used 3.89GiB
          devid    1 size 4.00GiB used 4.00GiB path /dev/xvdx
          devid    2 size 4.00GiB used 832.00MiB path /dev/xvdy
        
        Btrfs v3.12
        root@m1-host:-#
    
    In the output above, all 4GiB of data has been written to `/dev/xvdx` while only about 832 MiB of space is used on `/dev/xvdy`
    
    The balancing of the data can be effected using the `btrfs balance` subcommand as shown below:
    
        root@m1-host:-# btrfs balance start /var/lib/docker
        Done, had to relocate 14 out of 14 chunks
        root@m1-host:-# btrfs fi show /var/lib/docker
        Label: none  uuid: c6542f8f-66b6-45da-9552-87c21d4956a6
          Total devices 2 FS bytes used 3.89GiB
          devid    1 size 4.00GiB used 1.91GiB path /dev/xvdx
          devid    2 size 4.00GiB used 2.72GiB path /dev/xvdy
        
        Btrfs v3.12
        root@m1-host:-#

### Summary

We were able to see how Btrfs' powerful features allows us to manage disk space issues and improve performance and resiliency through the inbuilt RAID support. BTRFS is an advanced and future generation file system and fits very well with Docker's way of working. Since Btrfs treats all disks as a logical volume, it is trivial to extend volumes using a single command and it takes effect without the need to restart docker or make any other configuration changes.

There are some excellent and more indepth articles at [Docker and Btrfs in practice](https://docs.docker.com/engine/userguide/storagedriver/btrfs-driver/) on docs.docker.com.
