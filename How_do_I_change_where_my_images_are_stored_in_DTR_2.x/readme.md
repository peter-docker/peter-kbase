---
type: kbase
version: 9
title: "How do I change where my images are stored in DTR 2.x?"
summary: "For example, the dtr-registry-000000000000 volume is where the DTR with replica id 000000000000 stores its registry data when using the filesystem backend. To store your images in an NFS directory, specify the NFS URL at install time or switch to an NFS location after install with the reconfigure subcommand. For example, to store the registry data at the /opt/registry/data location on the host filesystem, run the install command including the --dtr-storage-volume argument:"
dateCreated: "Thu, 21 Jul 2016 22:02:03 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "programmerq"
platform:
testedon:
  - dtr-2.0.0
  - dtr-2.1.0
  - dtr-2.2.1
tags:
product:
  - EE
---

### DTR 2.1.x and newer

Docker Trusted Registry stores its data in a volume that corresponds with its replica id. For example, the `dtr-registry-000000000000` volume is where the DTR with replica id `000000000000` stores its registry data when using the filesystem backend.

To store your images in an NFS directory, specify the NFS URL at install time or switch to an NFS location after install with the `reconfigure` subcommand.

Details on this procedure are found in the docs:

* For DTR 2.2.x, see [https://docs.docker.com/datacenter/d...l-storage/nfs/](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/configure/external-storage/nfs/ "https://docs.docker.com/datacenter/dtr/2.2/guides/admin/configure/external-storage/nfs/").
* For DTR 2.1.x see [https://docs.docker.com/datacenter/d...igure/use-nfs/](https://docs.docker.com/datacenter/dtr/2.1/guides/configure/use-nfs/ "https://docs.docker.com/datacenter/dtr/2.1/guides/configure/use-nfs/").

To switch to a non-NFS storage location, you can specify the filesystem path of a manually-created volume name at install or reconfigure time. For example, to store the registry data at the `/opt/registry/data` location on the host filesystem, run the `install` command including the `--dtr-storage-volume` argument:

    docker run --rm -it docker/dtr install --dtr-storage-volume <^>/opt/registry/data<^^> ...

The `dtr-registry-<^>xxxxxxxxxxxx<^^>` container will end up with `/opt/registry/data` as a BIND mount instead of using the `dtr-registry-<^>xxxxxxxxxxxx<^^>` volume that the installer would otherwise create.

It is also possible to specify a volume instead of a host directory. For example, to specify a volume called `registrydata` on a DTR node, use the `--dtr-storage-volume` argument:

    docker run --rm -it docker/dtr install --dtr-storage-volume <^>registrydata<^^> ...

The `--dtr-storage-volume` option is also available when running docker/dtr reconfigure, so you can change the location that DTR uses after installation.

### DTR 2.0.x

Docker Trusted Registry (DTR) shows a path such as `/var/lib/docker/volumes/dtr-registry-<^>000000000000<^^>/_data` in what appears to be an editable text field, but it is grayed out. This text box is informational only. You can mount a filesystem at this location to store images in a different location.

To store your files at a different location:

1. Use SSH to connect to the node running the DTR replica.
2. Run the following command to find a volume called `dtr-registry-<^>$REPLICA_ID`<^^>, where <^>$REPLICA_ID<^^> corresponds to your twelve digit random hexadecimal DTR replica ID: 
    
        docker volume ls
3. Verify the proper location: 
    
        docker inspect dtr-registry-$REPLICA_ID
    You should see a mount point listed. This should correspond with the grayed out value in the GUI.
4. Mount your desired location as this location. 
    
    For example, you can use NFS to mount a different filesystem to this location.
    
        mount -t nfs server:/path/on/server /var/lib/docker/volumes/dtr-registry-$REPLICA_ID/_data
    
    To use another location, use a BIND mount. For example:
    
        mount -o bind /path/on/system /var/lib/docker/volumes/dtr-registry-$REPLICA_ID/_data

5. Once done, restart the Docker service on your system. Once it comes back up, your DTR images should now be served from this new location.

6. Make sure to add the mount point into your `/etc/fstab` so that they are still available on next boot.
