---
type: policy
version: 56
title: "Compatibility Matrix"
summary: "Docker EE is validated and supported to work in specific operating environments as outlined in the Docker Compatibility Matrix, adhere to the Docker Maintenance Lifecycle, and is supported within the defined Docker Scope of Support and Docker Commercial Support Service Levels."
dateCreated: "Tue, 22 Mar 2016 17:39:18 GMT"
dateModified: "Mon, 25 Sep 2017 20:12:30 GMT"
dateModified_unix: 1506370350
upvote: 26
internal: no
author: "justinnevill"
platform:
testedon:
tags:
product:
---

[Docker Enterprise Edition](https://www.docker.com/enterprise-edition "https://www.docker.com/enterprise-edition") is a subscription of software, support, and certification for enterprise dev and IT teams building and managing critical apps in production at scale. Docker EE provides a modern and trusted platform for all apps with integrated management and security across the app lifecycle, and includes three main technology components: the Docker daemon (fka "Engine"), Docker Trusted Registry (DTR), and Docker Universal Control Plane (UCP). Docker EE is validated and supported to work in specific operating environments as outlined in the **Docker Compatibility Matrix**, adhere to the [Docker Maintenance Lifecycle](/Policies/Maintenance_Lifecycle "Maintenance Lifecycle v2"), and is supported within the defined [Docker Scope of Support](/Policies/Scope_of_Support "Scope of Support") and [Docker Commercial Support Service Levels](/Policies/Commercial_Support_Service_Levels "Commercial Support Service Levels"). Refer to the [Subscription Services](https://www.docker.com/subscription-services "https://www.docker.com/subscription-services") or the [End User Subscription Agreement](https://www.docker.com/docker-software-end-user-subscription-agreement "https://www.docker.com/docker-software-end-user-subscription-agreement") for more information. To view the latest updates and upgrade instructions, visit the release notes for [daemon](https://docs.docker.com/docker-trusted-registry/cs-engine/release-notes/release-notes/ "https://docs.docker.com/docker-trusted-registry/cs-engine/release-notes/release-notes/"), [DTR](https://docs.docker.com/docker-trusted-registry/release-notes/release-notes/ "https://docs.docker.com/docker-trusted-registry/release-notes/release-notes/"), and [UCP](https://docs.docker.com/ucp/release_notes/ "https://docs.docker.com/ucp/release_notes/").

Versions on this page marked as "EOL" are unsupported, see the [Maintenance Lifecycle](/Policies/Maintenance_Lifecycle "Maintenance Lifecycle") page for more information on supported lifecycles.

* * *

## Docker Daemon
|                  | **CSE 1.12.z** | **CSE 1.13.z**      | **EE 17.03**        | **EE 17.06**        |
|------------------|:--------------:|:-------------------:|:-------------------:|:-------------------:|
| UCP 2.0.z (EOL)  | &#10003;       | &#9888;<sup>2</sup> | &#9888;<sup>2</sup> | &#10007;            |
| UCP 2.1.z        | &#10007;       | &#10003;            | &#10003;            | &#9888;<sup>2</sup> |
| UCP 2.2.z        | &#10007;       | &#10007;            | &#10007;            | &#10003;            |
| Host Platform    | On x86_64 Linux: <ul><li>CentOS 7.0, 7.1, 7.2</li> <li>RHEL 7.0, 7.1, 7.2</li> <li>SLES 12</li> <li>Ubuntu 14.04 LTS</li></ul> | On x86_64 Linux: <ul><li>CentOS 7.0, 7.1, 7.2, 7.3</li> <li>RHEL 7.0, 7.1, 7.2, 7.3</li> <li>SLES 12</li> <li>Ubuntu 14.04 LTS, 16.04 LTS</li></ul> | On x86_64 Linux: <ul><li>CentOS 7.1, 7.2, 7.3</li> <li>Oracle Linux 7.3<sup>4</sup></li> <li>RHEL 7.1, 7.2, 7.3</li> <li>SLES 12</li> <li>Ubuntu 14.04 LTS, 16.04 LTS</li></ul> | On x86\_64 Linux: <ul><li>CentOS 7.1, 7.2, 7.3, 7.4</li> <li>Oracle Linux 7.3<sup>4</sup></li> <li>RHEL 7.1, 7.2, 7.3, 7.4</li> <li>SLES 12</li> <li>Ubuntu 14.04 LTS, 16.04 LTS</li></ul> On x86\_64 Windows: <ul><li>Windows Server 2016<sup>3</sup></li></ul> On IBM Z (s390x) Linux<sup>3,5</sup>: <ul><li>RHEL 7.3, 7.4</li> <li>SLES 12</li> <li>Ubuntu 16.04</li></ul> |
| Storage Driver | <ul><li>CentOS: devicemapper</li> <li>RHEL: devicemapper</li> <li>SLES: btrfs</li> <li>Ubuntu: aufs3</li></ul> | <ul><li>CentOS: devicemapper</li> <li>RHEL: devicemapper</li> <li>SLES: btrfs</li> <li>Ubuntu: aufs3</li> | <ul><li>CentOS: devicemapper</li> <li>Oracle Linux: devicemapper</li> <li>RHEL: devicemapper</li> <li>SLES: btrfs</li> <li>Ubuntu: aufs3</li></ul> | <ul><li>CentOS: devicemapper</li> <li>Oracle Linux: devicemapper</li> <li>RHEL: devicemapper, overlay2<sup>6</sup></li> <li>SLES: btrfs</li> <li>Ubuntu: aufs3</li></ul> |
| Security | <ul><li>Apparmor</li> <li>Seccomp</li> <li>SELinux</li></ul> | <ul><li>Apparmor</li> <li>Seccomp</li> <li>SELinux</li></ul> | <ul><li>Apparmor</li> <li>Seccomp</li> <li>SELinux</li></ul> | <ul><li>Apparmor</li> <li>Seccomp</li> <li>SELinux<sup>5</sup></li></ul> |

**Notes**
1. UCP 1.1 is not compatible with CS Engine 1.12 when using the built-in swarm mode.
2. Supported only for the purpose of [upgrading to a later release](/article/Upgrading_to_Docker_17.03_(or_DDC_on_Engine_1.13)).
3. Only Docker EE Basic, or as UCP worker nodes with Docker EE Standard or Advanced when UCP manager nodes are hosted on another supported host platform.
4. Docker running on Oracle Linux is only supported with Red Hat Compatible kernel (RHCK) 3.10.0-514 or higher. Running Docker on older kernels and any version of the Unbreakable Enterprise Kernel is not supported.
5. SELinux policies for Docker containers are not supported on Docker EE running on IBM Z.
6. Beginning with Docker EE daemon version 17.06.2-ee-5 the overlay2 storage driver is supported, and is the recommended storage driver.


## Universal Control Plane (UCP)
|                       | **2.0.z (EOL)**       | **2.1.z**                      | **2.2.z**                      |
|-----------------------|:---------------------:|:------------------------------:|:------------------------------:|
| CSE 1.12.z            | &#10003;              | &#10007;                       | &#10007;                       |
| CSE 1.13.z            | &#9888;<sup>2</sup>   | &#10003;                       | &#9888;<sup>2</sup>            |
| EE Daemon 17.03       | &#9888;<sup>2</sup>   | &#10003;                       | &#9888;<sup>2</sup>            |
| EE Daemon 17.06       | &#10007;              | &#9888;<sup>2</sup>            | &#10003;                       |
| DTR 2.1.z (EOL)       | &#10003;              | &#10003;                       | &#10007;                       |
| DTR 2.2.z             | &#10003;              | &#10003;<sup>3</sup>           | &#10007;                       |
| DTR 2.3.z             | &#10007;              | &#10003;                       | &#10003;<sup>4</sup>           |
| DTR 2.4.z             | &#10007;              | &#10003;                       | &#10003;<sup>4</sup>           |
| Network Plugins       | Bridge, Host, Overlay | Bridge, Host, MACVLAN, Overlay | Bridge, Host, MACVLAN, Overlay |
| Volume Plugins        | Host                  | Host                           | Host                           |
| Compose File Versions | 2                     | 2, 3, and 3.1                  | 2, 3, 3.1, 3.2                 |

**Notes**
1. UCP 1.1 is not compatible with CS Engine 1.12 when using the built-in swarm mode.
2. Supported only for the purpose of [upgrading to a later release](/article/Upgrading_to_Docker_17.03_(or_DDC_on_Engine_1.13) "Upgrading to Docker Datacenter on Engine 1.13").
3. UCP 2.2.2 is compatible with DTR 2.2.z.
4. UCP 2.2.0 is not compatible with versions of DTR other than DTR 2.3.0. Please upgrade to UCP 2.2.2 to ensure compatibility to other versions of DTR 2.3.z and DTR 2.4.z.


## Docker Trusted Registry (DTR)
|                   | **2.1.z (EOL)** | **2.2.z**            | **2.3.z**                       |       **2.4.z**      |
|-------------------|:---------------:|:--------------------:|:-------------------------------:|:--------------------:|
| UCP 2.0.z (EOL)   | &#10003;        | &#10003;             | &#10007;                        | &#10007;             |
| UCP 2.1.z         | &#10003;        | &#10003;             | &#10003;<sup>1</sup>            | &#10003;             |
| UCP 2.2.z         | &#10007;        | &#10003;<sup>2</sup> | &#10003;<sup>2</sup>            | &#10003;<sup>2</sup> |
| Host Architecture | x86_64 Linux    | x86_64 Linux         | x86_64 Linux                    | <ul><li>x86_64 Linux</li> <li>IBM Z (s390x) Linux</li></ul> |
| Storage Drivers   | <ul><li>Local filesystem</li> <li>Amazon S3</li> <li>Google Cloud Storage</li> <li>Microsoft Azure</li> <li>NFS</li> <li>Openstack Swift</li> <li>S3 Compatible Storage: IBM Cloud Object Storage (fka Cleversafe), and Scality S3 Server</li></ul> | <ul><li>Local filesystem</li> <li>Amazon S3</li> <li>Google Cloud Storage</li> <li>Microsoft Azure</li> <li>NFS</li> <li>Openstack Swift</li> <li>S3 Compatible Storage: IBM Cloud Object Storage (fka Cleversafe), Minio Cloud Storage, and Scality S3 Server</li></ul> | <ul><li>Local filesystem</li> <li>Amazon S3</li> <li>Google Cloud Storage</li> <li>Microsoft Azure</li> <li>NFS</li> <li>Openstack Swift</li> <li>S3 Compatible Storage: IBM Cloud Object Storage (fka Cleversafe), Minio Cloud Storage, Scality S3 Server</li></ul> | <ul><li>Local filesystem</li> <li>Amazon S3</li> <li>Google Cloud Storage</li> <li>Microsoft Azure</li> <li>NFS</li> <li>Openstack Swift</li> <li>S3 Compatible Storage: IBM Cloud Object Storage (fka Cleversafe), Minio Cloud Storage, Scality S3 Server</li></ul> |

**Notes**
1. Only DTR verions 2.3.2 and greater are compatible with UCP 2.1.z.
2. DTR 2.3.z and 2.4.z are only compatible with UCP 2.2.2 or greater.
