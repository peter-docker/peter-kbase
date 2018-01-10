---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Why am I having network problems after firewalld is restarted?
internal: no             
comment: ""
type: kbase               
author:  lmuchnick
product:      
  - ee         
testedon:           
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.4
platform:           
  - linux
tags:               
  - daemon
  - error
  - networking
  - security
---

Linux hosts use a kernel module called __*iptables*__ to manage access to network devices including routing, port forwarding, and network address translation (NAT).

Docker modifies iptables rules when you start or stop containers, when you create or modify networks, when you attach containers to the network or other containers, and when you perform other network-related operations.

Typically, iptables rules are created by an initialization script or a daemon process such as __*firewalld*__. The rules do not persist across a system reboot, so the script or utility must run after every system reboot.

When firewalld is started or restarted, it removes the DOCKER chain from iptables, preventing Docker from working properly.  When using systemd, firewalld is started before Docker. If you start or restart firewalld after Docker, you need to restart the Docker daemon to enable the iptables rules again.
