---
type: reference
version: 1
title: "An Introduction to Storage Solutions for Docker CaaS"
summary: "Docker Datacenter provides a pluggable architecture approach for implementing storage of choice. This document explores three distinct storage tiers — graph driver storage, volume storage, and registry storage."
dateCreated: "Wed, 31 Aug 2016 15:54:18 GMT"
dateModified: "2018-01-09T17:58:05-06:00"
dateModified_unix: 1515542285
upvote: 4
internal: no
author: "bgrissin"
platform:
testedon:
  - cse-1.12.1-cs1
tags:
  - storage
product:
uniqueid: KB000186
---

TESTING TESTING TESTING AND MORE TESTING!!!

## Introduction

The Docker Datacenter Container as a Service (CaaS) platform delivers a secure, managed application environment for developers to build, ship, and run enterprise applications and custom business processes. This CaaS solution often requires storage across the different phases of the Software Delivery Supply Chain.

A variety of storage solutions exist for enterprise use, and a rapidly growing container ecosystem continues to provide many more storage solutions for future consideration. The growing ecosystem of new storage options combined with a need to utilize existing storage essentially brings forth the critical requirement that storage must be highly adaptable and configurable to achieve the optimal platform for containerized workloads.

The Docker Datacenter CaaS platform provides a default prescriptive storage configuration ‘out of the box.’ This allows consumers to begin building, shipping, and running containerized applications quickly.

However, and perhaps most importantly, Docker Datacenter CaaS provides a pluggable “batteries included, but replaceable” architecture that allows for the implementation and configuration of storage solutions that best meet your requirements across the entire Software Delivery Supply Chain. This pluggable architecture approach for implementing storage of choice also includes the ability to interchange other critical enterprise infrastructure services in a pluggable fashion such as networking, logging, authentication, authorization, and monitoring.

## Overview

Docker separates storage use cases within Docker Datacenter CaaS into three categories:

* **Docker image run storage (graph drivers)**
    
    Storage used for reading *image filesystem layers* from a running container state typically require IOPS and other read/write intensive operations, which leads to performance being a key storage service metric. These higher performance disk metrics often have higher costs and reduced scalability, so features such as redundancy or resiliency are sometimes traded off to manage the storage economics.

* **Persistent data container storage (volumes)**
    
    Containers often require *persistent storage* for using, capturing, or saving data beyond the container life cycle. Utilizing volume storage is best to keep data for future use or permit shared consumption by other containers or services. The many volume storage solutions available provide features such as high availability, performance, sharing, and reliable read/write filesystem protocols that are supported by Docker, OS vendors, and storage vendors.

* **Registry "at rest" image storage (registry)**
    
    When images are stored at rest on disk for cataloging and e-discovery purposes (as is the case for *Docker Trusted Registry*), key storage service metrics will likely revolve around scalability, costs, redundancy, and resiliency. In the case of DTR, acute attention will likely be toward lower cost per terabyte and higher scalability Lower IOPS and perhaps fewer filesystem protocols are often traded out in exchange for lower costs.

Each storage tier has specific storage requirements to achieve expected service levels across the different stages of the Software Delivery Supply Chain of a Docker Datacenter CaaS platform. Speed, scalability, high availability, recoverability, and costs are just a few of the many storage metrics that can help determine the optimal storage choice for each phase where storage is consumed.

This document will explore each of these three distinct storage tiers — *graph driver storage*, *volume storage*, and *registry storage* — in further detail.

## Graph Driver Storage

On each Docker Datacenter CaaS host, graph drivers interface local storage with Docker Engine. Performance is almost always considered the key metric for graph driver storage. OS support and resiliency are almost always required as well.

Graph drivers must be able to act as a local registry to store and retrieve copies of image layers that make up full images. Graph drivers also act as a caching mechanism to improve storage efficiency and download times for images within the local registry. They provides a Copy on Write (CoW) filesystem which is appended to a set of read-only image layers that constitute a running container. In acceptable IO time frames, graph drivers must also be able to reliably read into memory the sets of image layers that make a running container on Docker Engine. Graph drivers arguably are the workhorse of all storage used within a Docker Datacenter CaaS. Choosing an incorrect graph driver or misconfiguration can significantly impact expected service levels of the entire Software Delivery Supply Chain.

![Graph Driver Diagram](./images/GraphDriver.png)

Graph Drivers also supply a writeable CoW (Copy on Write) image layer on top of read-only filesystem layers of an image that are started as a running container. The CoW filesystem created at image runtime is assigned a unique filesystem layer ID, and this unique CoW layer ID does not persist or stay with the original image after each iteration of that image being run as a container. The default execution of an image as a container is ephemeral, meaning the container runtime does not automatically persist the CoW layer as part of the original image. You can save a running container with its unique CoW layer into a new image where the CoW is then transformed to an additional read-only layer on top of the original running read image layers. While it is possible to save the CoW contents of a running container as a new image itself, doing so as a means to persist data isn’t scalable, pragmatic, or practical to pass data. The CoW filesystem is most often successfully utilized when used as a means to expand or iterate a current image state to include necessary components or code to expand the image service requirements into a repeatable and reusable image.

Currently, the only supported graph drivers available are built into Docker CS Engine. CS Engine is the commercially-supported version of Docker Engine. Its code base is identical to the open source Docker Engine, but it has a limited set of supported configurations. Thus, the host OS has some influence on available, supported graph drivers. There is growing interest in experimental support using pluggable volumes, but none are currently recommended or available.

### Selecting Graph Drivers

Several factors can influence the selection of a graph driver. However, there are at least two factors that must be kept in mind before trying to decide:

* Currently no single graph driver is well suited for every use case
* Graph drivers are improving and evolving all of the time

With these factors in mind, the following guidelines should provide some direction when selecting graph drivers:

1. **Stability**
    
    * For the most stable and hassle-free Docker experience use the default graph driver for your distribution. When Docker is installed, a default graph driver is selected and installed based on the configuration of your system. Straying from this default may increase your chances of encountering bugs and other issues. However, staying with a default configuration may not always provide the best available solution to enhance your environment to reach optimum capabilities.
    
    * Follow the configuration specified in the [Compatibility Matrix](https://success.docker.com/Policies/Compatibility_Matrix). The Docker CS Engine set of supported configurations use the most stable and mature graph drivers. Straying from these configurations may also increase your chances of encountering bugs and issues.

2. **Experience and Expertise**
    
    * Choose a graph driver that you and your team/organization have experience with. For example, if you use RHEL or one of its downstream forks, you may already have experience with LVM and Device Mapper. If you do not feel you have expertise with any of the storage drivers supported by Docker, and you want an easy-to-use stable Docker experience, you should consider using the default driver installed by your distribution’s Docker package.

3. **Future-Proofing**
    
    * Many people consider OverlayFS as the future of of the Docker storage driver. There are two versions of overlayFS drivers available, Overlay and Overlay2. Overlay is only available for Docker Engine 1.11 and prior while Overlay2 is available for Docker Engine 1.12 and higher. Overlay is available in the 3.18 kernel and higher. Overlay2 is only recommended for use with the 4.0 kernel. Implementation options for OverlayFS on most Linux distributions are rather limited at the time of this publication release.
    
    * Overlay has known documented issues with inode exhaustion and commit exhaustion. OverlayFS is currently less mature and potentially less stable and tested than some of the more mature drivers such as AUFS and Device Mapper. For this reason, you should use the OverlayFS driver with caution and expect to encounter more bugs than if you were using a more mature driver.

## Volume Storage

Volume storage is an extremely versatile storage solution that can be used to do many things. Generally, volume storage provides ways for an application or user to store data generated by a running container. It extends beyond the life or boundaries of an existing or running container. This storage use case is commonly referred to as persistent storage. Persistent storage is an extremely important use case, especially for things like databases, images, file and folder sharing, and big data collection activities. Volume storage can also be used to do other interesting things such as provide easy access to secrets (for example, backed by KeyWhiz) or provide configuration data to a container from a key/value store. No matter where the data comes from, this information is translated by the volume driver plugin from the backend into a filesystem that can be accessed by normal tools meant to interact with the filesystem.

![Volume Driver Diagram](./images/VolumeDriver.png)

Many enterprises consume storage from various storage systems such as SAN and NAS arrays. These solutions often provide increased performance and availability as well as advanced storage features such as thin provisioning, duplication, encryption, and compression. They usually offer storage monitoring and management.

Volume drivers are used to connect storage solutions to the Docker Datacenter CaaS. You can use existing drivers or write drivers to allow the underlying storage to interface with the Docker APIs and Docker Datacenter.

A variety of volume driver solutions exist and can be plugged into and consumed within Docker Datacenter CaaS. There are volume storage projects from the open source community, and there are commercially-supported volume drivers available from storage vendors. Many volume driver plugins available today are software-defined and provide feature sets that are agnostic to the underlying physical storage.

> For a list of volume plugins, go to [https://docs.docker.com/engine/extend/legacy_plugins/](https://docs.docker.com/engine/extend/legacy_plugins/ "https://docs.docker.com/engine/extend/legacy_plugins/").

## Registry Storage

Registry storage is the backing storage for a running image registry instance such as Docker Trusted Registry or Docker Hub. Docker Trusted Registry is an on-premises image registry service within Docker Datacenter CaaS. Docker Hub is the public SaaS image registry option offered within Docker Cloud. Image registry backing storage, regardless of location, does not typically require high IO performance metrics, but they almost always require resiliency, scalability, and low cost economics to meet expected SLAs. The public Docker Hub image registry service is a specific example where these metric choices are clearly identifiable. Docker Hub requirements for faster push and pull speeds are secondary to metrics such as scalability, resiliency, and economics. Combined, these three metrics enable Docker to most efficiently manage and support a public image registry service that has over 20 millions pull requests a day, over 5 billion pull requests over a 3+ year period, and almost a half million images stored at any one given time. There are several available on-premises, supported backing storage options for Docker Trusted Registry such as Swift, S3, Azure, and Local/Network Filesystem(NFS). These storage options can provide the same registry SLAs required by large scale operations like Docker Hub. Local storage is also an available backing storage option and is the "out of the box" default for a Docker DataCenter CaaS platform. Ultimately, local file share storage options cannot offer similar or improved service levels in the order of magnitude that object storage can provide due to filesystem restrictions such as inode limits or filesystem protocol restrictions.

Docker images are immutable, read only, and attached with metadata. These digital characteristics relate very well with object storage features offered in S3, Azure, etc. Object storage also provides many additional digital management features that can enhance the overall image storage experience in addition to what Docker Trusted Registry provides for managing your application images. For example, object storage can provide additional service catalog items such as multi-dc or multi-region image replication to support Disaster Recovery and Continuous Availability designs, or offer additional built-in native redundancy for enhanced image availability, backup and restore solutions, or common API capabilities. It can also provide encryption at rest and client side encryption. Because of these additional features and advantages that object storage solutions provide, it's recommended that Docker Trusted Registry be configured to utilize an object storage backing solution for highly available installations. If HA is not required, then a single local filesystem is prepared as the default backing storage configuration for DTR, but this configuration can be changed to use a backing storage solution of your choice.

Often NFS or NFS-like file share solutions are used as an alternative backing storage solution to Object Storage. These file share based solutions can also fulfill the backing storage requirement for High Availability of Docker Trusted Registry. Because many enterprises are very familiar with NFS or similar shared filesystem storage solutions, there are natural tendencies to use shared filesystems over object storage solutions. There are disadvantages of using NFS or comparable shared filesystems as the backing storage for your Docker images:

1. NFS has many guarantees that erodes performance with too much unnecessary synchronization.
2. NFS essentially mimics a filesystem, often masking low level errors because it has no way to propagate them.
3. NFS doesn’t possess a native security model within.

![Registry Storage Diagram](./images/registrystorage.png)

Plugin storage options are not currently supported for registry storage, but there are a number of on-premises, S3 compliant backing storage options that are also a good fit for Docker Trusted Registry. Many partners leverage built-in S3-compliant API compatibility support as a way to have their storage service supported without having to write new code. There are also multiple shareable filesystem protocols other than NFS that are supported options such as ZFS-share on Ubuntu 16.04 LTS or btrfs on SUSE Linux Enterprise Server 12.

When planning a production-grade installation of Docker Trusted Registry on-premises, it's best to configure the image registry service as a highly available and redundant service, making the ability to change the backing storage of choice an imperative feature of Docker Datacenter CaaS DTR. All highly available DTR configurations do require a backing storage solution that can support a clustered set of containers requesting parallel asynchronous write requests to the physical storage itself. DTR does not assume, manage, or control any write-locking mechanisms that takes place for image/filesystem data being written to the physical underlying storage. Therefore, writes must be managed independently by the storage protocol of the backing storage itself.

## Conclusion

When choosing storage solutions for Docker Datacenter CaaS, consider the following:

1. Typically, containers are described and categorized to run as one of two separate, distinct states — *stateful* and *stateless*. A *stateful* service holds requirements for capturing and storing data during a container runtime. For example, a database that presents database tables on physical storage within a container for the purpose of writing or updating data is considered a stateful service. *Stateless* containers do not possess the same requirements for data persistence within a container runtime. Any data captured or generated during runtime does not exist or does not have to be recorded. When planning for image builds, it is recommended that you determine the state of each application or service within a container. Then, determine what the storage requirements are as well as the consumption possibilities for each container within the application stack. Also determine what storage options are available or have to be purchased to provide the best results.

2. Consider implementing a service catalog that captures all labels, filters, and constraints assigned to your storage hosts. Capturing all of these definitions will help eliminate overlap or isolate resource gaps in your Docker Datacenter CaaS environment and allow you to model and plan out the most efficient and informed orchestration methods available across the entire Software Delivery Supply Chain.

3. Regardless of container state, every container possesses a writeable Copy on Write (CoW) filesystem that is presented by each Docker host. As previously covered, graph drivers are not designed to accommodate data persistence and sharing, thus misinterpretation and misuse of this space as a means to persist and reuse data can lead to unexpected results and data loss. Strongly consider early in your planning stages that applications should not use this writeable space as a means to persist data beyond the container life cycle.

## Additional Resources

Check out these additional resources for more information:

* [Whitepaper: Understanding Docker Data Storage and Persistence](https://www.docker.com/sites/default/files/Understanding-Docker-Data-Storage-WP-rev3.pdf)
* [Understanding images, containers, and storage drivers](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers)
* [Configuring Storage for Docker Trusted Registry](https://docs.docker.com/docker-trusted-registry/configure/config-storage/)
* [Configuring Pluggable Volume Storage](https://github.com/docker/docker/blob/master/docs/extend/plugins.md)
* [Configuring Docker Graph Drivers](https://docs.docker.com/engine/userguide/storagedriver/selectadriver/)
* [Volume Plugins](https://docs.docker.com/engine/extend/plugins/#/volume-plugins)
* [The Definitive Guide To Docker Containers](https://www.docker.com/sites/default/files/WP-%20Definitive%20Guide%20To%20Containers.pdf)
* [Docker Trusted Registry backing storage using btrfs](/article/Working_with_Docker_on_btrfs_as_the_backend_storage_filesystem)
* [Swarm Filters](https://docs.docker.com/swarm/scheduler/filter/#/swarm-filters)
* [Labeling Images](https://docs.docker.com/engine/reference/builder/#/label)

