---
type: kbase
title: "Recommended steps for upgrading UCP or DTR"
summary: "When upgrading UCP or DTR between minor versions, you can’t skip versions. You can upgrade from any patch version of the previous minor version to any patch version of the current minor version. When upgrading between major versions, you must upgrade one major version at a time, and you have to upgrade to the earliest available minor version."
internal: no
author: "jacquesg"
platform:           # Optional. Keep all that apply.
  - linux
  - windows server
  - z systems
testedon:
- [UCP 1.1 to UCP 2.1]
- [UCP 2.1 to UCP 2.2]

tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - upgrading
product:
  - EE
---
## Issue

When upgrading UCP or DTR between minor versions, you can’t skip versions. You can upgrade from any patch version of the previous minor version to any patch version of the current minor version. When upgrading between major versions, you must upgrade one major version at a time, and you have to upgrade to the earliest available minor version.

Given a version number `X.Y.Z` - ( **MAJOR.MINOR.PATCH** ):

1. **MAJOR** version - incompatible API changes
2. **MINOR** version - add functionality in a backwards-compatible manner
3. **PATCH** version - backwards-compatible bug fixes

**Downgrading is not supported.**

## Prerequisites

Before performing an upgrade make sure you understand and follow the following requirements:

* [DTR upgrade](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/upgrade/ "https://docs.docker.com/datacenter/dtr/2.2/guides/admin/upgrade/")
* DTR uses [semantic versioning](http://semver.org/) and it aims to achieve specific guarantees while upgrading between versions.
* [UCP upgrade](https://docs.docker.com/datacenter/ucp/2.2/guides/admin/install/upgrade/)

## Recommended Approach

When upgrading DTR, `dtr remove` the replicas down to a single replica, perform the upgrade, and `dtr join` the replicas back to the full amount.

This will exponentially reduce the amount of issues that you will likely hit due to overlay issues. 

- If upgrading from UCP 1.1, you should skip UCP 2.0 and go directly to UCP 2.1. 
- If upgrading to UCP 2.2, you must upgrade to UCP 2.1 first though.

If `dtr-rethink-xxx` is constantly "restarting", do the following :
1. Stop the DTR Docker engines all at the same time (clears the state of `dtr-ol`) 
2. Start the DTR Docker engines back up one by one.
3. If `dtr-rethink` still restarting then go to UCP GUI, select all the `dtr-rethink` containers, and restart them at the same time.

## What's Next

If DTR upgrade fails with a `Failed to execute phase2` error refer to the [DTR upgrade fails](https://success.docker.com/article/DTR_upgrade_fails) article.

