---
type: kbase
version: 6
title: "DTR upgrade fails"
summary: "When upgrading between minor versions, you can’t skip versions, but you can upgrade from any patch versions of the previous minor version to any patch version of the current minor version. When upgrading between major versions you also have to upgrade one major version at a time, but you have to upgrade to the earliest available minor version."
dateCreated: "Fri, 28 Jul 2017 04:14:38 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "jacquesg"
platform:
  - linux
tags:
  - upgrading
  - error
testedon:
  - DTR 2.1.4 to DTR 2.2.4
  - DTR 2.2.4 to DTR 2.2.5
product:
  - EE
---

When upgrading DTR fails, the following errors might appear.
```
    FATA[0897] Failed to replace containers: failed to wait for rethink to be healthy Failed to wait for replica to settle on table blob_repository in db dtr2: gorethink: Timed out while waiting for tables. in:
    r.DB("dtr2").Table("blob_repository").Wait(timeout=600, wait_for="all_replicas_ready")
    ERRO[0939] Upgrade has failed. Try running it again.    
    FATA[0939] Failed to execute phase2: Phase 2 returned non-zero status: 1
```
## Prerequisites

Tested on below scenarios:

* [DTR 2.1.4 to DTR 2.2.4]
* [DTR 2.2.4 to DTR 2.2.5]

Before performing an upgrade make sure you understand and follow the following requirements:

* [DTR upgrade](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/upgrade/ "https://docs.docker.com/datacenter/dtr/2.2/guides/admin/upgrade/")
* DTR uses [semantic versioning](http://semver.org/) and it aims to achieve specific guarantees while upgrading between versions.

**Downgrading is not supported.**

Given a version number `X.Y.Z` - ( **MAJOR.MINOR.PATCH** ):

1. **MAJOR** version - incompatible API changes
2. **MINOR** version - add functionality in a backwards-compatible manner
3. **PATCH** version - backwards-compatible bug fixes

**Upgrades are supported according to the following rules:**

* When upgrading from one patch version to another you can skip patch versions because no data migration is done for patch versions.
* When upgrading between minor versions, you can’t skip versions, but you can upgrade from any patch versions of the previous minor version to any patch version of the current minor version.
* When upgrading between major versions you also have to upgrade one major version at a time, but you have to upgrade to the earliest available minor version. It is also strongly recommend that you upgrade to the latest minor/patch version for your major version first.

| Description | From | To | Supported |
| --- | --- | --- | --- |
| patch upgrade | x.y.0 | x.y.1 | yes |
| skip patch version | x.y.0 | x.y.2 | yes |
| patch downgrade | x.y.2 | x.y.1 | no |
| minor upgrade | x.y.* | x.y+1.* | yes |
| skip minor version | x.y.* | x.y+2.* | no |
| minor downgrade | x.y.* | x.y-1.* | no |
| skip major version | x._._ | x+2._._ | no |
| major downgrade | x._._ | x-1._._ | no |
| major upgrade | x.y.z | x+1.0.0 | yes |
| major upgrade skipping minor version | x.y.z | x+1.y+1.z | no |

## Resolution

**If upgrading DTR fails with any of the above errors, restart the DTR nodes 1 by 1 and run upgrade again:**

1. Restart one of the DTR nodes and *wait for it to come back up and running*.
2. Make sure the DTR UI can be accessed before moving to next step.
3. Repeat steps 1 and 2 until all DTR nodes have been restarted.
4. Run upgrade command again, replacing the version you want to upgrade to:

    ```
    docker run -it --rm \ 
    docker/dtr<^>:X:Y:Z<^^> upgrade \ 
    --ucp-insecure-tls
    ```
