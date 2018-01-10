---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to extract images from DTR volume backup
internal: no             # set to yes to keep it internal-only
comment: ""
type: kbase               # set to customerservice if applicable
author:  adamancini
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.4
platform:           # Optional. Keep all that apply.
  - linux
  - mac
  - windows
  - windows server
  - z systems
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - backup
  - storage
---

## Issue

Sometimes it may be necessary to extract the images from a DTR volume backup.  In the absence of a functioning DTR cluster, it's possible to extract these images using the open source `registry` image.

## Prerequisites

Before performing these steps, you must meet the following requirements:

- Ensure you have a recent backup of a DTR volume

## Resolution

1. First, identify the registry volume that needs to be extracted:

```
# docker volume ls --filter=name=dtr
DRIVER              VOLUME NAME
local               dtr-ca-a2382528a00b
local               dtr-postgres-a2382528a00b
local               <^>dtr-registry-a2382528a00b<^^>
local               dtr-rethink-a2382528a00b
```

2. Next, navigate to the `/var/lib/docker/volumes` directory and copy the contents of the volume to a known location for easy access.

```
# cd /var/lib/docker/volumes/
# cp -r <^>dtr-registry-a2382528a00b<^^>/ /<^>backup-directory<^^>/.
```

3. Next, create a Docker volume to hold the image data.  In this example, a volume called `registry` is created:

```
# docker volume create <^registry<^^>
```

4. Next, copy the image data from the DTR volume into the volume just created:

```
# cp -r /<^>backup-directory<^^>/<^>dtr-registry-a2382528a00b<^^>/_data/ /var/lib/docker/volumes/<^>registry<^^>/
```

5. Now perform a quick check to ensure that the repositories are in place:

```
# tree -L 6 /var/lib/docker/volumes/<^>registry<^^>
registry
└── _data
    └── docker
        └── registry
            └── v2
                ├── blobs
                │   └── sha256
                └── repositories
                    └── mike
```

6. Next, start up a container using the `registry:2` image provided by Docker, and mount the named volume created in the container at `/var/lib/registry`:

```
# docker run -v <^>registry<^^>:/var/lib/registry -d -p 5000:5000 --name <^>registry<^^> registry:2
```

7. Now attempt to pull an image from the registry:

```
# docker pull localhost:5000/mike/minecraft
Using default tag: latest
latest: Pulling from mike/minecraft
Digest: sha256:f3c567d7a45bd7ef4ef442ec18842f05c056943662d70c3100fa032253fd3c84
Status: Image is up to date for localhost:5000/mike/minecraft:latest
```


## What's Next

- [DTR Backups and Recovery](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/backups-and-disaster-recovery/)
- [Monitor DTR for problems](https://docs.docker.com/datacenter/dtr/2.3/guides/admin/monitor-and-troubleshoot/)
