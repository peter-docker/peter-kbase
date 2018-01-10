---
type: kbase
version: 6
title: "Can UCP run in a boot2docker virtualbox?"
summary: "The problem with this is that the Docker Machine virtualbox driver does not provide a way for the guest boot2docker VM to _specify_ what IP address it should have. One approach might be to manually provision VMs in virtualbox and then use the docker-machine utility to install Docker using the generic driver. Be sure to pass in the static, host-only network IP addresses to the UCP tool when installing/joining using the --host-address option."
dateCreated: "Tue, 26 Jul 2016 22:07:14 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "programmerq"
platform:
testedon:
tags:
product:
  - EE
---

No, running a UCP cluster in a boot2docker virtualbox is not supported and not recommended.

## Why not?

UCP must run on [Docker CS Engine](https://docs.docker.com/cs-engine/ "Docker CS Engine"). CS Engine does not run on the boot2docker distro that Docker Machine's virtualbox and vmwarefusion drivers use.

The second problem is the networking model that the Docker Machine virtualbox driver uses. Each virtualbox VM that Docker Machine provisions has two network interfaces. The first one is the machine's eth0, which uses NAT mode networking. The other network interface each VM gets is attached to a virtualbox host-only network. Multiple VMs can be on this network. The problem with this is that the Docker Machine virtualbox driver does not provide a way for the guest boot2docker VM to _specify_ what IP address it should have. You may be able to install a UCP cluster across multiple VMs on this host-only network. The problem happens when these VMs reboot.

UCP cares very much about what IP address it is using. Almost every component needs to be aware of this IP and the IPs of other UCP nodes. Because of this, running UCP in an environment without a permanent IP address is very difficult. The DHCP service associated with this host-only networking model in virtualbox doesn't guarantee the same VM will get the same IP. Instead, it hands out IPs based on first come-first-serve basis. This race condition can and will break your UCP cluster during a future reboot.

Due to this combination of factors, running a UCP cluster in this sort of environment is not supported and not recommended.

## Alternatives

Workarounds or alternatives will include a variety of options, but the two most important pieces will be: 1) supported distro running Docker CS Engine and 2) more stable networking configuration.

Here's one specific example of an alternate approach that should satisfy these requirements. We can still use virtualbox, and we can still use Docker Machine, but not the Docker Machine virtualbox driver. One approach might be to manually provision VMs in virtualbox and then use the `docker-machine` utility to install Docker using the generic driver.

An example acceptable process would include using Ubuntu 14.04 VMs with the following:

1. NAT and host mode networking are appropriate here. The key difference is that the **VMs should be configured with static IPs** instead of DHCP IP addresses on the host-only network. Ideally, it would be **attached to a host-only network** instead of the one used by Docker Machine to avoid conflicts.
2. This should have **passwordless sudo user only** accessible via SSH with an SSH key.
3. At this point, utilize `docker-machine` to install Docker and set up Docker TLS: 
    
        docker-machine create -d generic --generic-ip-address <^><ip><^^> --generic-ssh-user <^><user><^^> --generic-ssh-key <^><path/to/key><^^> --engine-install-url https://packages.docker.com/<^>1.11<^^>/install.sh 
        <name>
    
    > Since the VMS were manually provisioned, instead of running `docker-machine`, alternately run the [https://packages.docker.com/1.11/install.sh](https://packages.docker.com/1.11/install.sh "https://packages.docker.com/1.11/install.sh") script to install CS Engine. It works similar to [https://get.docker.com/](https://get.docker.com/ "https://get.docker.com/") but installs Docker CS Engine.
4. Finally, install UCP. Be sure to pass in the static, host-only network IP addresses to the UCP tool when installing/joining using the `--host-address` option.
