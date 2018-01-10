---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: UCP requires IPv4 IP Forwarding 
internal: no             
comment: ""
type: kbase               
author:  darwintraver
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
  - ee-17.06.2-ee-5
  - ucp-2.2.3
platform:           
  - linux
tags:               
  - daemon
  - error
  - installing
  - networking

---
## Issue

UCP manager election process can be impacted if IPv4 forwarding is disabled, which can cause UCP stability issues. This is required by UCP for networking to function properly.

The following WARN level error message can be found in the Docker engine logs if it is not enabled:

```
level=warning msg="IPv4 forwarding is disabled. Networking will not work"
```

In addition, the output of `docker info` from the host OS will display a WARNING at the bottom:

```
WARNING: IPv4 forwarding is disabled
```

## Prerequisites

Before performing these steps, you must meet the following requirements:

-Universal Control Plane 2.0.x and higher
-Docker Engine 17.0.x
-RHEL/CentOS 7.x
-Ubuntu 14.0x/16.0x

## Resolution

To enable IPv4 IP forwarding:

1. Check the current value:

     ```
    /sbin/sysctl net.ipv4.conf.all.forwarding
    ```
2. Enable the setting:

    ```
    /sbin/sysctl -w net.ipv4.conf.all.forwarding=1
    ```
3. Check the output of `docker info`:

    ```
    docker info
    ```
4. To preserve this setting post-reboot, please refer to the `/etc/sysctl.conf` notes:

    ```
    sysctl settings are defined through files in
    /usr/lib/sysctl.d/, /run/sysctl.d/, and /etc/sysctl.d/.

    Vendors settings live in /usr/lib/sysctl.d/.
    To override a whole file, create a new file with the same in
    /etc/sysctl.d/ and put new settings there. To override
    only specific settings, add a file with a lexically later
    name in /etc/sysctl.d/ and put new settings there.

    For more information, see sysctl.conf(5) and sysctl.d(5).
    ```

## What's Next

More details can be found in the following Docker documentation link:
[https://docs.docker.com/engine/userguide/networking/default_network/container-communication/](https://docs.docker.com/engine/userguide/networking/default_network/container-communication/)
