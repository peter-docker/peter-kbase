---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: NFS options during DTR installation
internal: no             # set to yes to keep it internal-only
comment: "https://github.com/docker/dhe-deploy/issues/6515"
type: kbase               # set to customerservice if applicable
author:  darwintraver
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.06.2-ee-3
  - ucp-2.2.3
  - dtr-2.3.0
platform:           # Optional. Keep all that apply.
  - linux
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - installing
  - storage

---
## Issue

During DTR installation with NFS storage, the installer might fail out if NFSv4 is required.

```
ERRO[0082] Failed to verify NFS: Problem running container 'dtr-nfs-test' from image 'docker/dtr:2.3.4': polling failed with 60 attempts 1s apart: Error response from daemon: Error response from daemon: error while mounting volume '/var/lib/docker/volumes/dtr-nfs-test/_data': error while mounting volume with options: type='nfs' device='<NFS path>' o='addr=<NFS IP Address>,rw,sync,actimeo=0': connection refused
```

## Prerequisites

- DTR 2.1.x-2.3.x
- NFSv3/4

## Resolution

It is currently not possible to pass custom NFS options when using the `--nfs-storage-url` switch for DTR installation/reconfiguration.

The folllowing is a workaround:

1. Create a Docker volume using `nfsvers=4` under the `--opt` switch
2. Install DTR using `--dtr-storage-volume` to reference the Docker volume just created
