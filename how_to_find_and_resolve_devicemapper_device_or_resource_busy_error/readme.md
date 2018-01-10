---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to find and resolve devicemapper "device or resource busy" error
internal: no             # set to yes to keep it internal-only
comment: "internal comments that will only show up in the GH source file"
type: kbase               # set to customerservice if applicable
author:  quaddo
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.03.2-ee-4
  - ucp-2.1.3
platform:           # Optional. Keep all that apply.
  - linux
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - daemon
  - error
  - logging
  - monitoring
---
## Issue

While trying to remove a container, you receive a `device or resource busy` error:

```
# docker rm <^>0bfafa146431<^^>
Error response from daemon: Unable to remove filesystem for 0bfafa146431771f6024dcb9775ef47f170edb2f1852f71916ba44209ca6120a: remove /app/docker/containers/0bfafa146431771f6024dcb9775ef47f170edb2f152f71916ba44209ca6120a/shm: device or resource busy
```

## Prerequisites

- Node must be running RHEL 7.3 or later, or CentOS 7
- Node must be configured to use devicemapper

## Resolution

1. From the error message, identify the device ID which appears to be blocking the filesystem removal. In the above example, the ID is:

  ```
  0bfafa146431771f6024dcb9775ef47f170edb2f152f71916ba44209ca6120a
  ```

2. Copy the following non-destructive script to the node:

  ```
  #!/bin/bash

  # A simple script to get information about mount points and pids and their
  # mount namespaces.

  if [ $# -ne 1 ];then
    echo "Usage: $0 <devicemapper-device-id>"
    exit 1
  fi

  ID=$1

  MOUNTS=`find /proc/*/mounts | xargs grep $ID 2>/dev/null`

  [ -z "$MOUNTS" ] &&  echo "No pids found" && exit 0

  printf "PID\tNAME\t\tMNTNS\n"
  echo "$MOUNTS" | while read LINE; do
    PID=`echo $LINE | cut -d ":" -f1 | cut -d "/" -f3`
    # Ignore self and thread-self
    if [ "$PID" == "self" ] || [ "$PID" == "thread-self" ]; then
      continue
    fi
    NAME=`ps -q $PID -o comm=`
    MNTNS=`readlink /proc/$PID/ns/mnt`
    printf "%s\t%s\t\t%s\n" "$PID" "$NAME" "$MNTNS"
  done
  ```

  _(Source: https://github.com/rhvgoyal/misc/blob/master/find-busy-mnt.sh )_

  Name the script `find-busy-mnt.sh` and set the execute bit:

  ```
  chmod +x find-busy-mnt.sh
  ```

3. Run the script, passing the ID above to it as an argument:

  ```
  ./find-busy-mnt.sh <^>0bfafa146431771f6024dcb9775ef47f170edb2f152f71916ba44209ca6120a<^^>
  ```

  The script should output the name of the process which is blocking the filesystem removal.

  If the process is (for example) `ntpd`, then stop the daemon, and retry the previous container removal.
