---
type: guide
version: 4
title: "Upgrade an entire cluster with CentOS, Docker Engine, UCP, and DTR"
summary: "The upgrade should happen in the following order: Engine and kernel, UCP, and then DTR. Please wait for the rebooted/upgraded DTR replica to become active and healthy in the cluster before proceeding. Once all the DTR replicas have been upgraded, proceed with the rest of the workers. The UCP upgrade will not interrupt the applications running within the cluster. Now that the OS, Engine, and UCP are updated, follow the main documentation for upgrading DTR."
dateCreated: "Thu, 03 Aug 2017 01:46:57 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "clemenko"
platform:
  - linux
testedon:
tags:
  - upgrading
product:
  - EE
---

## Introduction

Upgrading an entire cluster can be difficult. This guide is intended to provide step-by-step instructions. The upgrade should happen in the following order: Engine and kernel, UCP, and then DTR. Details to follow.

## Prerequisites

The guide assumes the following:

* All nodes are CentOS based.

* All nodes have Internet access.

* All nodes are healthy and online.

Obviously there are dozens of permutations of OS and networking. The step order and ideas can easily be applied in most environments. It should be noted that a complete cluster upgrade of OS, Engine, UCP, and DTR should be performed during a maintenance window. The maintenance window should prevent any changes from jeopardizing the upgrade results.

## Step 1 - Upgrade Engine and OS

Before starting please note there is **NO PROMOTION** or **DEMOTION** of managers and workers! Because of the timing, this can cause a loss of quorum resulting in the need to restore. The restore procedures will require extra time, SO take it slow and be methodical.

**Starting with the first Manager :**

1. Stop Docker Engine. `sudo systemctl stop docker`
2. Upgrade the OS and Engine. `sudo yum update -y` If there was a kernel update you will need to reboot the node and skip step 3.
3. Start Docker Engine. `sudo systemctl start docker`
4. Verify the Manager node is up and healthy. On node : `sudo docker node ls` and verify the manager is healthy.  
    
    **Please wait for the rebooted/upgraded manager to become active and healthy in the cluster before proceeding. **

**Then repeat with the next Manager if it exists. **

Once all the managers have been upgraded, proceed with the DTR servers.

**Continuing with the first DTR replica :**

1. Stop Docker Engine. `sudo systemctl stop docker`
2. Upgrade the OS and Engine. `sudo yum update -y` If there was a kernel update you will need to reboot the node and skip step 3.
3. Start Docker Engine. `sudo systemctl start docker`  
    
    **Please wait for the rebooted/upgraded DTR replica to become active and healthy in the cluster before proceeding.**

**Then repeat with the next DTR replica if it exists. **

Once all the DTR replicas have been upgraded, proceed with the rest of the workers.

**Next we can proceed with the rest of the Workers :** Let's assume a graceful action.

1. From a **Manager** node: `sudo docker node update --availability drain $NODE_NAME`
2. From the **Worker **node, stop Docker Engine. `sudo systemctl stop docker`
3. Upgrade the OS and Engine. `sudo yum update -y` If there was a kernel update you will need to reboot the node and skip step 4.
4. If node was not rebooted, start Docker Engine. `sudo systemctl start docker`
5. If the node was rebooted, from a **Manager** node: `sudo docker node update --availability active $NODE_NAME`

## Step 2 - Upgrade UCP

Now that the OS and Engine are updated, follow the main documentation for upgrading UCP. The UCP upgrade will not interrupt the applications running within the cluster. It is also worth noting that NO configuration changes should be made during the upgrade.

[https://docs.docker.com/datacenter/ucp/2.1/guides/admin/upgrade/#upgrade-ucp](https://docs.docker.com/datacenter/ucp/2.1/guides/admin/upgrade/#upgrade-ucp "https://docs.docker.com/datacenter/ucp/2.1/guides/admin/upgrade/#upgrade-ucp")

## Step 3 - Upgrade DTR

Now that the OS, Engine, and UCP are updated, follow the main documentation for upgrading DTR. Please keep in mind the DTR upgrade will prevent push and pull functions to complete.

[https://docs.docker.com/datacenter/dtr/2.2/guides/admin/upgrade/#step-1-upgrade-dtr-to-21-if-necessary](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/upgrade/#step-1-upgrade-dtr-to-21-if-necessary "https://docs.docker.com/datacenter/dtr/2.2/guides/admin/upgrade/#step-1-upgrade-dtr-to-21-if-necessary")

## What's Next?

The next step should be to perform another backup of UCP and DTR. Ideally save the new backups under a different version name. Please do follow the documentation closely.

* UCP backup procedures : [https://docs.docker.com/datacenter/ucp/2.1/guides/admin/backups-and-disaster-recovery/#backup-command](https://docs.docker.com/datacenter/ucp/2.1/guides/admin/backups-and-disaster-recovery/#backup-command "https://docs.docker.com/datacenter/ucp/2.1/guides/admin/backups-and-disaster-recovery/#backup-command")
* DTR backup procedures : [https://docs.docker.com/datacenter/dtr/2.2/guides/admin/backups-and-disaster-recovery/#back-up-dtr-data](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/backups-and-disaster-recovery/#back-up-dtr-data "https://docs.docker.com/datacenter/dtr/2.2/guides/admin/backups-and-disaster-recovery/#back-up-dtr-data")

If you run into trouble at any point in the guide please feel free to contact support, forums, or our slack channels.
