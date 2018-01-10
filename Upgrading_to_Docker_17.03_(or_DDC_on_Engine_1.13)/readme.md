---
type: kbase
version: 5
title: "Upgrading to Docker 17.03 (or DDC on Engine 1.13)"
summary: "For example, upgrading to CS Engine 1.11.z would require you to be currently running CS Engine 1.10.z, and likewise upgrading to DTR 2.1.z would require you to be currently using DTR 2.0.z. Thus, if you are currently running CS Engine 1.11.z and UCP 1.1.z, Docker recommends skipping CS Engine 1.12.z and UCP 2.0.z and instead upgrading directly to CS Engine 1.13.z and UCP 2.1.z."
dateCreated: "Tue, 14 Feb 2017 16:41:20 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "justinnevill"
platform:
testedon:
  - cse-1.13.1-cs1
  - ee-17.03.0-ee-1
tags:
  - upgrading
product:
  - EE
---

As written in the [documentation](https://docs.docker.com/datacenter/ucp/2.1/guides/admin/upgrade/ "https://docs.docker.com/datacenter/ucp/2.1/guides/admin/upgrade/"), when upgrading to Docker 17.03 or Docker Engine 1.13, the components must be upgraded in a specific order: first upgrade CS Engine on all nodes, followed by UCP, followed by DTR.

The upgrade process for each component usually requires you to upgrade sequentially from one release to the next. For example, upgrading to CS Engine 1.11.z would require you to be currently running CS Engine 1.10.z, and likewise upgrading to DTR 2.1.z would require you to be currently using DTR 2.0.z. This means that if you wanted to jump two versions of a product, you would usually have to sequentially upgrade using the versions in between.

In the specific cases of Docker 17.03 and CS Engine 1.13 (UCP 2.1.z and DTR 2.2.z), however, there are some exceptions which allow you to skip some sequential upgrades. Here are the cases:

* **CS Engine**: You **can** upgrade directly from CS Engine 1.11.z to CS Engine 1.13.z (skipping 1.12.z). You can also upgrade from Engine 1.12.z to 1.13.z, as usual.
* **UCP**: You **can** upgrade directly from UCP 1.1.z to UCP 2.1.z (skipping 2.0.z). You can also upgrade from UCP 2.0.z to 2.1.z, as usual.
* **DTR**: You **cannot** skip upgrades with DTR. You must upgrade from DTR 2.1.z to 2.2.z. If you are currently on DTR 2.0.z, you need to upgrade first to 2.1.z and then to 2.2.z.

Docker CS Engine 1.13 has several architectural improvements that provide increased stability. Thus, if you are currently running CS Engine 1.11.z and UCP 1.1.z, Docker recommends skipping CS Engine 1.12.z and UCP 2.0.z and instead upgrading directly to CS Engine 1.13.z and UCP 2.1.z. Please note that this guideline is for **the specific case** of upgrading to Engine 1.13 and UCP 2.1. In future versions, you can expect that upgrades should be done with sequential versions, as usual.

As always, Docker recommends using the latest ".z" patch versions within a given release in order to ensure the highest possible stability, security, and quality.

> There is known issue in UCP versions 2.1.0, 2.1.1, and 2.1.2 where upgrading from UCP 1.1.z can cause swarm to leave worker nodes in a pending state: `[Pending] Completing node registration.`
> 
> There are two workarounds for rectifying this issue:
> 
> A. When upgrading from UCP 1.1.z, first upgrade to UCP 2.0.z, and then to UCP 2.1.z. This will prevent the issue from occurring in the first place and is the recommended upgrade path.
> 
> B. If you have already upgraded from UCP 1.1.z directly to UCP 2.1.z, you can rectify the issue by restarting the ucp-swarm-manager container on each of your UCP controller nodes.
> 
> This known issue will be fully rectified in the UCP 2.1.3 patch release.
