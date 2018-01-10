---
type: policy
version: 43
title: "Maintenance Lifecycle"
summary: "Docker EE is validated and supported to work in specific operating environments as outlined in the Docker Compatibility Matrix, adhere to the Docker Maintenance Lifecycle, and is supported within the defined Docker Scope of Support and Docker Commercial Support Service Levels."
dateCreated: "Tue, 23 Aug 2016 14:45:18 GMT"
dateModified: "Mon, 25 Sep 2017 17:49:19 GMT"
dateModified_unix: 1506361759
upvote: 0
internal: no
author: "justinnevill"
platform:
testedon:
tags:
product:
---

[Docker Enterprise Edition](https://www.docker.com/enterprise-edition "https://www.docker.com/enterprise-edition") is a subscription of software, support, and certification for enterprise dev and IT teams building and managing critical apps in production at scale. Docker EE provides a modern and trusted platform for all apps with integrated management and security across the app lifecycle, and includes three main technology components: the Docker daemon (fka "Engine"), Docker Trusted Registry (DTR), and Docker Universal Control Plane (UCP). Docker EE is validated and supported to work in specific operating environments as outlined in the [Docker Compatibility Matrix](/Policies/Compatibility_Matrix "Compatibility Matrix"), adhere to the **Docker Maintenance Lifecycle**, and is supported within the defined [Docker Scope of Support](/Policies/Scope_of_Support "Scope of Support") and [Docker Commercial Support Service Levels](/Policies/Commercial_Support_Service_Levels "Commercial Support Service Levels"). Refer to the [Subscription Services](https://www.docker.com/subscription-services "https://www.docker.com/subscription-services") or the [End User Subscription Agreement](https://www.docker.com/docker-software-end-user-subscription-agreement "https://www.docker.com/docker-software-end-user-subscription-agreement") for more information. To view the latest updates and upgrade instructions, visit the release notes for [daemon](https://docs.docker.com/docker-trusted-registry/cs-engine/release-notes/release-notes/ "https://docs.docker.com/docker-trusted-registry/cs-engine/release-notes/release-notes/"), [DTR](https://docs.docker.com/docker-trusted-registry/release-notes/release-notes/ "https://docs.docker.com/docker-trusted-registry/release-notes/release-notes/"), and [UCP](https://docs.docker.com/ucp/release_notes/ "https://docs.docker.com/ucp/release_notes/").

This page shows all currently supported versions and the most recently EOL'd version. Versions that are older than those shown are also EOL.

#### Important Definitions
* **"Major Releases" (X.y.z)** are vehicles for delivering major and minor feature development and enhancements to existing features. They incorporate all applicable Error corrections made in prior Major Releases, Minor Releases, and Maintenance Releases.
* **"Minor Releases" (x.Y.z)** are vehicles for delivering minor feature developments, enhancements to existing features, and defect corrections. They incorporate all applicable Error corrections made in prior Minor Releases, and Maintenance Releases.
* **"Maintenance Releases" (x.y.Z)** are vehicles for delivering Error corrections that are severely affecting a number of customers and cannot wait for the next major or minor release. They incorporate all applicable defect corrections made in prior Maintenance Releases.
* **"End of Life" (EOL)** versions are no longer supported by Docker, updating to a later version is recommended.


## Docker Daemon
|   | **CSE 1.11.z** | **CSE 1.12.z** | **CSE 1.13.z** | **EE 17.03** | **EE 17.06** |
|---|----------------|----------------|----------------|--------------|--------------|
| General Availability (GA) | 2016-04-27 (1.11.1-cs1) | 2016-09-21 (1.12.1-cs1) | 2017-02-08 (1.13.1-cs1) | 2017-03-02 (17.03) | 2017-08-16 (17.06) |
| End of Life (EOL) | **EOL as of 2017-03-02** | 2017-11-14 | 2018-02-07 | 2018-03-01 | 2018-08-15 |
| Release Frequency | Aligned with open source Docker Engine X.Y releases | Aligned with open source Docker Engine releases | Aligned with open source Docker Engine releases | Quarterly | Quarterly |
| Supported Lifespan | Current and Y-2 releases | Current and Y-2 releases | Current and Y-2 releases | 1 year from release | 1 year from release |
| Patches and Update Frequency | n/a | As Needed:<ul><li>Maintenance Releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> | As Needed:<ul><li>Maintenance Releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> | As Needed:<ul><li>Maintenance Releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> | As Needed:<ul><li>Maintenance Releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> |


## Universal Control Plane (UCP)
|                           | **2.0.z**                | **2.1.z**           | **2.2.z**           |
|---------------------------|--------------------------|---------------------|---------------------|
| General Availability (GA) | 2016-11-10 (2.0.0)       | 2017-02-08 (2.1.0)  | 2017-08-16 (2.2.0)  |
| End of Life (EOL)         | **EOL as of 2017-08-16** | 2018-02-07          | 2018-08-15          |
| Release Frequency         | X.Y Quarterly            | X.Y Quarterly       | X.Y Quarterly       |
| Supported Lifespan        | Current and Y-1 releases | 1 year from release | 1 year from release |
| Patches and Update Frequency | As Needed:<ul><li>Maintenance Releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> | As Needed:<ul><li>Maintenance Releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> | As Needed:<ul><li>Maintenance Releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> |


## Docker Trusted Registry (DTR)
|                           |         **2.1.z**        |      **2.2.z**      |      **2.3.z**      |      **2.4.z**      |
|---------------------------|--------------------------|---------------------|---------------------|---------------------|
| General Availability (GA) | 2016-11-10 (2.1.0)       | 2017-02-08 (2.2.0)  | 2017-08-16 (2.3.0)  | 2017-11-02 (2.4.0)  |
| End of Life (EOL)         | **EOL as of 2017-08-16** | 2018-02-07          | 2018-08-15          | 2018-11-01          |
| Release Frequency         | X.Y Quarterly            | X.Y Quarterly       | X.Y Quarterly       | X.Y Quarterly       |
| Supported Lifespan        | Current and Y-1 releases | 1 year from release | 1 year from release | 1 year from release |
| Patches and Update Frequency | As Needed:<ul><li>Maintenance releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> | As Needed:<ul><li>Maintenance releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> | As Needed:<ul><li>Maintenance releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> | As Needed:<ul><li>Maintenance releases</li><li>Security patches</li><li>Customer hotfixes</li></ul> |
