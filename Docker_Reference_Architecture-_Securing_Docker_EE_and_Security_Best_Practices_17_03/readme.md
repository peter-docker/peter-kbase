---
type: reference
version: 1.1.1
title: "Docker Reference Architecture: Securing Docker EE and Security Best Practices (EE 17.03)"
summary: "This document outlines the default security of Docker EE 17.03 as well as best practices for further securing it. New features such as image signing and verification with Docker Content Trust and secrets are also explored."
dateCreated: "Fri, 10 Mar 2017 03:26:31 GMT"
dateModified: "Thu, 31 Aug 2017 04:34:51 GMT"
dateModified_unix: 1504154091
internal: no
author:
  - clemenko
  - pvnovarese
platform:
  - linux
  - windows server
testedon:
  - ee-17.03.0-ee-1
  - dtr-2.2.1
  - ucp-2.1.0
  - cse-1.13.1-cs1
tags:
  - security
product:
  - EE
---

## Introduction

Docker lives by “Secure by Default.” With Docker Enterprise Edition (Docker EE), the default configuration and policies provide a solid foundation for a secure environment. However, they can easily be changed to meet the specific needs of your organization.

Docker focuses on three key areas of container security: *secure access*, *secure content*, and *secure platform*. This results in having isolation and containment features not only built into Docker EE but also enabled out of the box. The attack surface area of the Linux kernel is reduced, the containment capabilities of the Docker daemon are improved, and you build, ship, and run safer applications.



## What You Will Learn

This document outlines the default security of Docker EE as well as best practices for further securing Universal Control Plane and Docker Trusted Registry. New features introduced in Docker CS Engine 1.13 such as image signing and verification with Docker Content Trust and secrets are also explored.

> *Note:* This document addresses Docker 17.03 or greater (including Docker CS Engine 1.13) and a Linux host OS with kernel 3.10 or greater.



## Prerequisites

* Docker EE 17.03 or Docker CS Engine 1.13
* Become familiar with [Docker Concepts from the Docker docs](https://docs.docker.com/engine/understanding-docker/).

## Abbreviations

The following abbreviations are used in this document:

* DDC = Docker Datacenter (UCP and DTR)
* UCP = Universal Control Plane
* DTR = Docker Trusted Registry
* DCT = Docker Content Trust
* RBAC = Role Based Access Controls
* CA = Certificate Authority
* CS = Docker Commercially Supported Engine
* HA = High Availability
* BOM = Bill of Materials
* CLI = Command Line Interface



## Engine and Node Security

There are already several resources that cover the basics of Docker Engine security.

* [Docker Security Documentation](https://docs.docker.com/engine/security/security/) covers the fundamentals, such as namespaces and control groups, the attack surface of the Docker daemon, and other kernel security features.
* [CIS Docker 1.13 Benchmark](https://benchmarks.cisecurity.org/tools2/docker/CIS_Docker_1.13.0_Benchmark_v1.0.0.pdf) covers the various security-related options in Docker Engine.
* [Docker Bench Security](https://github.com/docker/docker-bench-security) is a script that audits your configuration of Docker Engine against the CIS Benchmark.



### Choice of Operating Systems

Docker CS Engine 1.13 (which is a required prerequisite for installing UCP and included with Docker EE) is supported on the following host operating systems:

* RHEL/CentOS/Oracle Linux 7.1/7.2/7.3 (YUM-based systems)
* Ubuntu 14.04 LTS, 16.04 LTS
* SUSE Linux Enterprise 12

For other versions, check out the [official Docker support matrix](https://success.docker.com/Policies/Compatibility_Matrix).

To take advantage of the built-in security configurations and policies, run the latest version Docker CS Engine. Also ensure that you update the OS with all of the latest patches. In all cases it is highly recommended to remove as much unnecessary software as possible.



### Limit Root Access to Node

Docker EE uses a completely separate authentication backend from the host, providing a clear separation of duties. Docker EE can leverage an existing LDAP/AD infrastructure for authentication. It even utilizes [RBAC Labels](#ucp-rbac) to control access to objects like images and running containers, meaning teams of users can be given full access to running containers. With this access, the users can watch the logs and execute a shell inside the running container. The user never needs to log into the host. Limiting the number of users that have access to the host reduces the attack surface.



### Remote Access to Daemon

Don't enable the remote daemon socket. If you must open it for Engine, then ALWAYS secure it with certs. When using Universal Control Plane, you should not open the daemon socket. If you must, be sure to review the [instructions for securing the daemon socket](https://docs.docker.com/engine/security/https/).



### Privileged Containers

Avoid running privileged containers if at all possible. Running a container privileged gives the container access to ALL the host namespaces (i.e. net, pid, and others). This gives full control of the host to the container. Keep your infrastructure secure by keeping the container and host authentication separate.



### Container UID Management

By default the user inside the container is root. Using a defense in depth model, it is recommended that not all containers run as root. An easy way to mitigate this is to use the `--user` declaration at run time. The container runs as the specified user, essentially removing root access.

Also keep in mind that the UID/GID combination for a file inside a container is the same outside of the container. In the following example, a container is running with a UID of 10000 and GID of 10000. If the user touches a file such as `/tmp/secret_file`, on a BIND-mounted directory, the UID/GID of the file is the same both inside and outside of the container as shown:

    root @ ~  docker run --rm -it -v /tmp:/tmp --user 10000:10000 alpine sh
    / $ whoami
    whoami: unknown uid 10000
    / $ touch /tmp/secret_file
    / $ ls -asl /tmp/secret_file 
         0 -rw-r--r--    1 10000    10000            0 Jan 26 13:48 /tmp/secret_file
    / $ exit
    root @ ~  ls -asl /tmp/secret_file 
    0 -rw-r--r-- 1 10000 10000 0 Jan 26 08:48 /tmp/secret_file

Developers should `root` as little as possible inside the container. Developers should create their app containers with the `USER` declaration in their Dockerfiles.



### Seccomp

> *Note:* Seccomp for Docker CS Engine is available starting with RHEL/CentOS 7 and SLES 12.

Seccomp (short for **Secure Computing Mode**) is a security feature of the Linux kernel, used to restrict the syscalls available to a given process. This facility has been in the kernel in various forms since 2.6.12 and has been available in Docker Engine since 1.10. The current implementation in Docker Engine provides a default set of restricted syscalls and also allows syscalls to be filtered via either a whitelist or a blacklist on a per-container basis (i.e. different filters can be applied to different containers running in the same Engine). Seccomp profiles are applied at container creation time and cannot be altered for running containers.

Out of the box, Docker comes with a default Seccomp profile that works extremely well for the vast majority of use cases. In general, applying custom profiles is not recommended unless absolutely necessary. More information about building custom profiles and applying them can be found in the [Docker Seccomp docs](https://docs.docker.com/engine/security/seccomp/).

To check if your kernel supports seccomp:

    cat /boot/config-`uname -r` | grep CONFIG_SECCOMP=

Look for the following in the output:

    CONFIG_SECCOMP=y



### AppArmor / SELinux

AppArmor and SELinux are similar to Seccomp in regards to use profiles. They differ in their execution though. The profile languages used by AppArmor and SELinux are different. AppArmor is only on Debian-based distributions such as Debian and Ubuntu. SELinux is available on Fedora/RHEL/CentOS/Oracle Linux. Rather than a simple list of system calls and arguments, both allow for defining actors (generally processes), actions (reading files, network operations), and targets (files, IPs, protocols, etc.). Both are Linux kernel security modules, and both support mandatory access controls (MAC).

They need to be enabled on the host, while SELinux can be enabled at the daemon level.

To enable SELinux in the Docker daemon modify `/etc/docker/daemon.json` and add the following:

    "selinux-enabled": true

> **Note** Remember that the file `daemon.json` is JSON-based and needs opening and closing braces — `{ }`.

To check to see if SELinux is enabled:

    docker info |grep -A 3 "Security Options"

You should see `selinux` in the output if it is enabled:

    Security Options:
     seccomp
      Profile: default
     selinux

AppArmor is not applied to the Docker daemon. Apparmor profiles need to be applied at container run time:

    docker run --rm -it --security-opt apparmor=docker-default hello-world

There are some good resources for installing and setting up AppArmor/SELinux such as:

* [Techmint - Implementing Mandatory Access Control with SELinux or AppArmor in Linux](http://www.tecmint.com/mandatory-access-control-with-selinux-or-apparmor-linux/)
* [nixCraft - Linux Kernel Security (SELinux vs AppArmor vs Grsecurity)](https://www.cyberciti.biz/tips/selinux-vs-apparmor-vs-grsecurity.html)

Bottom line is that you should always use AppArmor or SELinux for their supported operating systems.



### Runtime Privilege and Linux Capabilities — Advanced Tooling

> *Starting with kernel 2.2, Linux divides the privileges traditionally associated with superuser into distinct units, known as capabilities, which can be independently enabled and disabled.* — [Capabilities man page](http://man7.org/linux/man-pages/man7/capabilities.7.html)

Linux capabilities are an even more granular way of reducing surface area. Docker Engine has a default list of capabilities that are kept for newly-created containers, and by using the `--cap-drop` option for `docker run`, users can exclude additional capabilities from being used by processes inside the container on a capability-by-capability basis. All privileges can be dropped with the `--user` option.

Likewise, capabilities that are by default not granted to new containers can be added with the `--cap-add` option, though this is discouraged unless absolutely necessary. Using `--cap-add=ALL` is highly discouraged.

More details can be found in the [Docker Run Reference](https://docs.docker.com/engine/reference/run/#/runtime-privilege-and-linux-capabilities).



### Controls from the CIS Benchmark

There are many good practices that you can apply from the [CIS Docker 1.13 Benchmark](https://benchmarks.cisecurity.org/tools2/docker/CIS_Docker_1.13.0_Benchmark_v1.0.0.pdf "Center for Internet Security Docker 1.13 Benchmark"). Some of the controls may not be applicable to your environment. To apply these controls, edit the Engine settings. Editing the Engine setting in `/etc/docker/daemon.json` is the best choice for most of these controls. Refer to the [daemon.json guide](https://docs.docker.com/engine/reference/commandline/dockerd/#/daemon-configuration-file) for details.

Let's look at some of them.



#### Apply Centralized Logging — CIS 1.13 Benchmark : Section 2.12

Having a central location for all Engine and container logs is recommended. This provides "off-node" access to all the logs, empowering developers without having to grant them SSH access.

To enable centralized logging, modify `/etc/docker/daemon.json` and add the following:

    "log-level": "syslog",  
    "log-opts": {syslog-address=tcp://192.x.x.x},

Then restart the daemon:

    sudo systemctl restart docker



#### Enable Content Trust — CIS 1.13 Benchmark : Section 4.5

Content Trust is the cryptographic guarantee that the image pulled is the correct image. Content Trust is enabled by Notary. [Signing images with Notary is discussed](#dtr-notary) later in this document.

First, you need some basic background. When transferring data among networked systems, trust is a central concern. In particular, when communicating over an untrusted medium such as the Internet, it is critical to ensure the integrity and the publisher of all the data a system operates on. You use Docker Engine to push and pull images (data) to a public or private registry. Content Trust gives you the ability to verify both the integrity and the publisher of all the data received from a registry over any channel. Content Trust is available from Docker Hub, DDC 2.0 (DTR 2.10) and higher, and Docker EE 17.03 and higher. To enable it, add the following shell variable:

    export DOCKER_CONTENT_TRUST=1



### Audit with Docker Bench

[Docker Bench Security](https://store.docker.com/community/images/docker/docker-bench-security) is a script that checks for dozens of common best practices around deploying Docker containers in production. The tests are all automated and are inspired by the [CIS 1.13 Benchmark](https://benchmarks.cisecurity.org/tools2/docker/CIS_Docker_1.13.0_Benchmark_v1.0.0.pdf "Center for Internet Security Docker 1.13 Benchmark").

Here is how to run it :

    docker run -it --net host --pid host --cap-add audit_control -v /var/lib:/var/lib \
      -v /var/run/docker.sock:/var/run/docker.sock -v /usr/lib/systemd:/usr/lib/systemd \
      -v /etc:/etc --label docker_bench_security docker/docker-bench-security

Here is an example output:

![](./images/bench-security.jpg)

The output is straightforward. There is a status message, CIS Benchmark Control number, and description fields. Look for the `[WARN]` messages. The biggest section to pay attention to is `1 - Host Configuration`. Keep in mind that this tool is designed to audit Docker Engine and is a good starting point. Docker Bench Security is NOT intended for auditing the setup of UCP/DTR. There are a few controls that, when enabled, break UCP and DTR.

The following controls are not needed because they affect the operation of UCP/DTR:

* 2.1 Restrict network traffic between containers — Needed for container communication
* 2.6 Configure TLS authentication for Docker daemon — Should not be enabled as it is not needed
* 2.8 Enable user namespace support — Currently not supported with UCP/DTR
* 2.15 Do not enable Swarm mode, if not needed — Swarm mode is the underlying orchestration framework
* 2.18 Disable Userland Proxy — Disabling the proxy affects how the routing mesh works



## UCP Security

Universal Control Plane is configured to be "Secure by default." It uses two Certificate Authorities with Mutual TLS. UCP sets up two CAs. One CA is used for ALL internal communication between managers and workers. The second CA is for the end user communication. Two communication paths are vital to keeping the traffic segregated. The use of Mutual TLS is automatic between the manager and worker nodes. Mutual TLS is where both the client and the server verify the identity of each other. Another important note is that the worker nodes are unprivileged, meaning they do not have access to the cluster state or secrets. When adding nodes to the UCP cluster you need to use a join token. The token itself incorporates a checksum of the CA cert so the new node can verify it's talking to the right swarm.

![](./images/UCP_Mutual_TLS.png)



### Networking

Networking can be an important part of a Docker EE deployment. The basic rule of thumb is not to have firewalls between the manager and worker nodes. When deploying to a cloud infrastructure, low latency is a must between nodes. Low latency ensures the databases are able to keep quorum. When a software, or hardware, firewall is deployed between the nodes the following ports need to be opened.

![](./images/ucp-ports.jpg)



### Authentication

Docker EE features a single sign-on for the entire DDC cluster, which is accomplished via shared authentication service for UCP and DTR. The single sign is provided out of the box with AuthN or via an externally-managed LDAP/AD authentication service. Both authentication backends provide the same level of control. When available, a corporate LDAP service can provide a smoother account experience for users. Refer to the [LDAP/AD configuration docs](https://docs.docker.com/datacenter/ucp/2.1/guides/admin/configure/external-auth/) and [Docker EE Best Practices and Design Considerations](https://success.docker.com/Architecture/Docker_Reference_Architecture%3A_Docker_EE_Best_Practices_and_Design_Considerations) for instructions and best practices while configuring LDAP authentication.

![](./images/ldap-integration-1.png)



### External Certificates

Using external certificates is a good option when integrating with a corporate environment. Using external, officially-signed certificates simplifies having to distribute Certificate Authority certificates. One best practice is to use the Certificate Authority (CA) for your organization. You can reduce the number of certificates by adding multiple Subject Alternative Names (SANs) to a single certificate. This allows the certificate to be valid for multiple URLs. For example, you can set up a certificate for `ucp.example.com`, `dtr.example.com`, and all the underlying hostnames and IP addresses. One certificate/key pair makes deploying certs easier.

To add an external certificate, go to **Admin Settings --> Certificates** in the UCP web interface and add the CA, Cert, and Key.

![](./images/ucp-cert-add.jpg)

More detailed [instructions for adding external certificates](https://docs.docker.com/datacenter/ucp/2.1/guides/admin/configure/use-your-own-tls-certificates/) are available in the Docker docs.



### Join Token Rotation

Depending on how you have built your DDC cluster, you may have the token stored in an insecure location. To alleviate any concerns, join tokens can be rotated once the cluster is built. To rotate the keys, go to the **Admin Settings --> Swarm Mode Parameters** page, expand the **Join Tokens** tick, and hit the **Rotate** button.

![](./images/token_rotation.jpg)



### Node Certificate Expiration

UCP's management plane uses a private CA and certificates for all internal communication. The client certificates are automatically rotated on a schedule. Key rotation is a strong method for reducing the effect of a compromised node. You have the option to reduce the time interval, with the default being 90 days. Shorter intervals add stress to the UCP cluster. Similar to rotating the join tokens, go to **Admin Settings --> Swarm Mode Parameters** and scroll down.

![](./images/cert_rotation.jpg)



### Client Certificate Bundles

Universal Control Plane makes it easy to create a client certificate bundle for use with the Docker client. The client bundle allows end users to create objects and deploy services from a local Docker client. To create a client bundle, log into UCP and click **Profile** in the upper right hand corner.

![](./images/dtr_client_bundles.jpg)

From here a client bundle can be created and downloaded. Inside the bundle are the files necessary for talking to the UCP cluster directly.

Navigate to the directory where you downloaded the user bundle, and unzip it.

    unzip ucp-bundle-admin.zip

Then run the `env.sh` script:

    eval $(<env.sh)

Verify the changes:

    docker info

The `env.sh` script updates the `DOCKER_HOST` environment variable to make your local Docker CLI communicate with UCP. It also updates the `DOCKER_CERT_PATH` environment variables to use the client certificates that are included in the client bundle you downloaded.

From now on, when you use the Docker CLI client, it includes your client certificates as part of the request to the Docker Engine. You can now use the Docker CLI to create services, networks, volumes, and other resources on a Swarm managed by UCP.

To stop talking to the UCP cluster run the following command:

    unset DOCKER_HOST DOCKER_TLS_VERIFY DOCKER_CERT_PATH

Run `docker info` to verify that you are talking to the local daemon.



### RBAC

Role Based Access Control is HIGHLY recommended for limiting user access. With RBAC, administrators have the ability to limit users' access to images and running containers. Universal Control Plane uses "teams" and "labels" to apply the different levels of permissions. Users are applied across both DTR and UCP, making management simple. Additionally, integration of LDAP/AD can use existing groups to populate teams.

RBAC for Docker EE has two fundamental types of users: administrators and regular users. Administrators can make changes to the UCP cluster, while regular users have permissions that range from no access to full control over volumes, networks, images, secrets, and containers. Administrators can choose one of four permission levels ranging from no access to full control as a default for regular users.

![](./images/rbac-levels.jpg)

Teams and labels give the administrator fine-grain control over permissions. Each team can have multiple labels. Each label has a key of `com.docker.ucp.access.label`. The label is then applied to the containers, services, networks, secrets, and volumes.

![](./images/team-levels.jpg)

Here is a better view of the permissions levels.

![](./images/permissions_ucp.png)

As you can see, labels are necessary for implementing RBAC. Since a team can have multiple labels, it is a good idea to be as descriptive as possible. Remember the label is going to be applied to the network, volumes, secrets, and even the running containers.

Here are some good label examples:

* Development team for the CRM production app: `com.docker.ucp.access.label=CRM_prod`
* Development team for the CRM development app: `com.docker.ucp.access.label=CRM_dev`
* Development Team for the Billing production app: `com.docker.ucp.access.label=Billing_prod`
* Development team for the Billing development app: `com.docker.ucp.access.label=Billing_dev`

All of the RBAC permissions are managed in the **User Management** tab. For example, a team called `crm` can be created:

![](./images/user_management.jpg)

A permission label for RBAC also needs to be created. Click on the new team name, and then go to the **Permissions** tab. Create **CRM_prod**, and set the permissions to **Full Control**.

![](./images/rbac_add.jpg)

Having separate labels within a team can provide logical separation between apps and even environments. For example, limit access the CRM team has to the objects labeled `CRM_prod`, and give full control to objects labeled `CRM_dev`.

![](./images/rbac_list.jpg)

Alternatively, you can use the same label across different teams providing different levels of access:

* Developer Team — **Restricted Control** --> `com.docker.ucp.access.label=awesome_app`
* Operations Team — **Full Control** --> `com.docker.ucp.access.label=awesome_app`
* IA/Security Team — **View Only** --> `com.docker.ucp.access.label=awesome_app`

This method gives different capabilities to objects that carry the label `com.docker.ucp.access.label=awesome_app`. Basically, any team can have its permission set on ANY Label. If the label is not added to that team, then they won't see the label or have access to the resources.



### Secrets

Secrets are new starting with Docker CS Engine 1.13. A *secret* is a blob of data such as a password, SSH private key, SSL certificate, or another piece of data that should not be transmitted over a network. Secrets are stored unencrypted in a Dockerfile or stored in your application's source code. In Docker CS Engine 1.13 and higher as well as Docker EE 17.03 and higher, you can use Docker secrets to centrally manage this data and securely transmit it to only those containers that need access to it. Secrets follow a Least Privileged Distribution model and are encrypted at rest and in transit in a Docker swarm. A given secret is only accessible to those services which have been granted explicit access to it and only while those service tasks are running.

Secrets requires a Swarm mode cluster. You can use secrets to manage any sensitive data which a container needs at runtime but you don't want to store in the image or in source control such as:

* Usernames and passwords
* TLS certificates and keys
* SSH keys
* Other important data such as the name of a database or internal server
* Generic strings or binary content (up to 500 kb in size)

> **Note**: Docker secrets are only available to Swarm services, not to standalone containers. To use this feature, consider adapting your container to run as a service with a scale of 1.

Another use case for using secrets is to provide a layer of abstraction between the container and a set of credentials. Consider a scenario where you have separate development, test, and production environments for your application. Each of these environments can have different credentials, stored in the development, test, and production swarms with the same secret name. Your containers only need to know the name of the secret to function in all three environments.

When you add a secret to the swarm, Docker sends the secret to the Swarm manager over a mutual TLS connection. The secret is stored in the Raft log, which is encrypted. The entire Raft log is replicated across the other managers, ensuring the same high availability guarantees for secrets as for the rest of the swarm management data.

![](./images/secret-keys.jpg)

When you grant a newly-created or running service access to a secret, the decrypted secret is mounted into the container in an in-memory filesystem at `/run/secrets/<secret_name>`. You can update a service to grant it access to additional secrets or revoke its access to a given secret at any time.

A node only has access to (encrypted) secrets if the node is a swarm manager or if it is running service tasks which have been granted access to the secret. When a container task stops running, the decrypted secrets shared to it are unmounted from the in-memory filesystem for that container and flushed from the node's memory.

If a node loses connectivity to the swarm while it is running a task container with access to a secret, the task container still has access to its secrets but cannot receive updates until the node reconnects to the swarm.

Docker EE's strong RBAC system can tie *secrets* into it with the exact same labels demonstrated before, meaning you should always limit the scope of each secret to a specific team. If you don't apply ANY labels, the default label is the owner.

For example, you can add TLS certificates as secrets. Using the same RBAC example teams as previously mentioned, the following example adds `ca.pem`, `cert.pub`, and `cert.pem` to the secrets vault. Notice the use of the label `com.docker.ucp.access.label=CRM_prod`. This is important for enforcing the RBAC rules. Also note the use of the team name in the naming of the secret. For another idea for updating or rolling back secrets, consider adding a version number or date to the secret name. This is made easier by the ability to control the mount point of the secret within a given container. This also prevents teams from trying to use the same secret name. The following adds the CA's public certificate in pem format as a secret named `CRM.ca.pem.v1`.

![](./images/tls-secret.jpg)

From the following page you can see permissions and what services are actively using this secret:

![](./images/secret-inspect.jpg)

Secrets are only available to services. The following creates an `nginx` service. The service and the secret MUST have the same permission label. If they don't match, UCP won't allow you to deploy.

![](./images/secret-create-1.jpg)

Click **Next**, and go to the **Environment** tab. Click the **+ Use a secret**. The advanced settings allow you to set the UID/GID and file mode for the secret when it is mounted. Binaries and tar balls can be added as secrets, with a file size up to 500KB.

![](./images/secret-create-2.jpg)

When using the CLI the option, `--secret source=,target=,mode=` needs to be added to the `docker service create` command as follows:

    docker service create \
    --secret source=CRM.ca.pem,target=ca.pem \ 
    --secret source=CRM.cert.pub,target=cert.pub \
    --secret source=CRM.cert.pem,target=cert.pem \
    -l com.docker.ucp.access.label=CRM_prod -p 443 --name nginx nginx

Notice that the secrets are mounted to `/run/secrets/`. Because of labels in our example, only administrators and the crm team have access to this container and secret.

![](./images/secret-run.jpg)

Changing secrets is as easy as removing the current version and creating it again. Please keep in mind that you need to be sure the labels on the new secret are correct.



### Logging

Since UCP is deployed as a containerized application, it uses the Engine logging configuration automatically. However, there is an easier way to configure logging across the cluster if you are using a syslog format. In **Admin Settings --> Logs** you can set the logging level and destination.

![](./images/ucp_logging.jpg)

This is a great way to point all the logs to Splunk or an ELK (Elasticsearch Logstash Kibana) stack.



## DTR Security

Docker Trusted Registry continues the "Secure by Default" theme with two new strong features: Image Signing (via the Notary project) and Image Scanning. Additionally, DTR shares authentication with UCP, which simplifies setup and provides strong RBAC without any effort.

DTR stores metadata and layer data in two separate locations. The metadata is stored locally in a database that is shared between replicas. The layer data is stored in a configurable location.



### External Certificates

Just as with UCP, DTR can use fully-signed company certificates or self-signed certs. You can use the Certificate Authority (CA) for your organization. To reduce the number of certificates, you can add multiple Subject Alternative Names (SANs) to a single certificate. This allows the certificate to be valid for multiple URLs. For example, when setting up a certificate for `ucp.example.com`, add SANs of `dtr.example.com` and all the underlying hostnames and IP addresses. Using this technique allows the same certificate to be used for both UCP and DTR.

External certificates are added to DTR by going to **Settings** --> **Domain** --> **Show TLS Settings**.

![](./images/dtr-cert-add.jpg)

For more instructions for adding external certificates, refer to the [Docker docs](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/configure/use-your-own-tls-certificates/).



### Storage Backend — S3 or NFS

The choice of storage backend for DTR has affects on both performance and security. Your choices are as follows:
TypeProsConsLocal FilesystemFast and Local. Pairs great with local block storage.Requires bare metal or ephemeral volumes. NOT good for HA.S3Great for HA and HTTPS communications. Several third party servers available. Can be encrypted at rest.Requires maintaining or paying for an external S3 compliant service.Azure Blob StorageCan be configured to act as a local but have redundancy within Azure Storage. Can be encrypted at rest.Requires Azure cloud account.SwiftSimilar to S3 being an object store.Requires OpenStack infrastructure for service.Google Cloud StorageSimilar to S3 being an object store. Can be encrypted at rest.Requires a Google Cloud account.NFSEasy to setup/integrate with existing infrastructure.Slower due to network calls.
To change the settings, go to **Settings** --> **Storage** in UCP.

![](./images/dtr-storage.jpg)

Your choice of storage is highly influenced by where you are deploying Docker EE because it is important to place DTR's backend storage as close as possible to DTR itself. You should always ensure that you are using HTTPS (TLS). Also consider how are you going to backup DTR's images. When in doubt, use a secure object store, such as S3 or similar. Object stores provide the best balance between security and ease of use and also make it easy for HA DTR setups.



### Garbage Collection

Garbage collection is an often-overlooked area from a security standpoint. Old, out-of-date images may contain security flaws or exploitable vulnerabilities, so removing unnecessary images is important. Garbage collection is a feature that ensures that unused images (and layers) are removed. Garbage collection is a substantial process, so it's best to run at times when the system is least utilized. Scheduling a regular collection time in **Settings** --> **Garbage Collection** is the best approach to ensuring unnecessary images are removed.

![](./images/dtr-garbage.jpg)



### Organizations — RBAC

Since Universal Control Plane and Docker Trusted Registry utilize the same authentication backend, users are shared between the two. This simplifies user management. However, there is a slight difference in how teams are handled. Universal Control Plane uses "Teams" to organize users. Docker Trusted Registry uses "organizations." This is due to how the repositories are handled. Similar to UCP's user management, organizations need to be created.

![](./images/dtr_org_create.jpg)

Once the organizations are created, DTR has the ability to add teams within the organization.

Here’s an overview of the permission levels available in DTR:

* Anonymous users: Can search and pull public repositories
* Users: Can search and pull public repos, and create and manage their own repositories
* Team member: Can do everything a user can do, plus has the permissions of the teams the user is member of
* Team admin: Can do everything a team member can do, and can also add members to the team
* Organization admin: Can do everything a team admin can do, can create new teams, and add members to the organization
* DDC admin: Can manage anything across UCP and DTR

![](./images/dtr_org_team_create.jpg)

For example, an organization named `crm`, a team named `prod`, and a repository named `crm/awesome_app` were created. Permissions can now be applied to the images themselves.

![](./images/dtr_repo_permissions.jpg)

This chart shows the different permission levels:

![](./images/dtr_team_permissions.jpg)

It is important to limit the number of users that have access to images. Applying the permission levels correctly is important.



### Content Trust/Image Signing — Notary

*Notary is a tool for publishing and managing trusted collections of content. Publishers can digitally sign collections and consumers can verify integrity and origin of content. This ability is built on a straightforward key management and signing interface to create signed collections and configure trusted publishers.*

Docker Content Trust/Notary provides a cryptographic signature for each image. The signature provides security so that the image you want is the image you get. If you are curious about what makes Notary secure, read about [Notary's Architecture](https://docs.docker.com/notary/service_architecture/). Since Docker EE is "Secure by Default," Docker Trusted Registry comes with the Notary server out of the box.

In addition, Docker Content Trust enables you to institute the concept of threshold signing and gating for the releases. Under this model, software is not released until all necessary parties (or a quorum) sign off. This can be enforced by requiring (and verifying) the needed signatures for an image. This policy ensures that the image has made it through the whole process: if someone tries to make it skip a step, the image will lack a necessary signature, thus preventing deployment of that image.

The following examples shows the basic usage of Notary. To use image signing, you need to create a repository in DTR and enable it on your local Docker Engine. First, enable the client, and sign an image:

    root @ ~  export DOCKER_CONTENT_TRUST=1
    root @ ~  docker tag alpine dtr.example.com/admin/alpine:signed 
    root @ ~  docker push dtr.example.com/admin/alpine:signed
    The push refers to a repository [dtr.example.com/admin/alpine]
    865e1c468a35: Layer already exists 
    e0cfcaccf697: Layer already exists 
    e2d4ee32e967: Layer already exists 
    60ab55d3379d: Layer already exists 
    signed: digest: sha256:131d77d4ccf5916a94d026e2f5865a2e2acefd56fc6debceb83e50cf24eb4e99 size: 1156
    Signing and pushing trust metadata
    You are about to create a new root signing key passphrase. This passphrase
    will be used to protect the most sensitive key in your signing system. Please
    choose a long, complex passphrase and be careful to keep the password and the
    key file itself secure and backed up. It is highly recommended that you use a
    password manager to generate the passphrase and keep it safe. There will be no
    way to recover this key. You can find the key in your config directory.
    Enter passphrase for new root key with ID 44d193b: 
    Repeat passphrase for new root key with ID 44d193b: 
    Enter passphrase for new repository key with ID 2a0738c (dtr.example.com/admin/alpine): 
    Repeat passphrase for new repository key with ID 2a0738c (dtr.example.com/admin/alpine): 
    Finished initializing "dtr.example.com/admin/alpine"
    Successfully signed "dtr.example.com/admin/alpine":signed

The above does the following:

* Enables Content Trust with `export DOCKER_CONTENT_TRUST=1`.
* Tags an image destined for DTR with `docker tag alpine dtr.example.com/admin/alpine:signed`
* Pushes the image with the tag "signed" : `docker push dtr.example.com/admin/alpine:signed`
* The Docker client starts the push to DTR. Since there is no local root key (certificate), one needs to be created with a passphrase. `Enter passphrase for new root key with ID 44d193b:`
* A repository key is created with a passphrase. `Enter passphrase for new repository key with ID f93c4a5 (dtr.example.com/admin/alpine):`

When re-pushing signed images to DTR, you do not have to create the keys again. You are prompted for the image passphrase.

    root @ ~  docker push dtr.example.com/admin/alpine:signed
    The push refers to a repository [dtr.example.com/admin/alpine]
    60ab55d3379d: Layer already exists 
    signed: digest: sha256:3952dc48dcc4136ccdde37fbef7e250346538a55a0366e3fccc683336377e372 size: 528
    Signing and pushing trust metadata
    Enter passphrase for repository key with ID 2a0738c: 
    Successfully signed "dtr.example.com/admin/alpine":signed

A successfully signed image has a green check mark in the DTR GUI.

![](./images/dtr-signed.jpg)



#### Key Management

Docker and Notary clients store state in its `trust_dir` directory, which is `~/.docker/trust` when enabling Docker Content Trust. This directory is where all the keys are stored. All the keys are encrypted at rest. It is VERY important to protect that directory with permissions.

The `root_keys` subdirectory within `private` stores root private keys, while `tuf_keys` stores targets, snapshots, and delegations private keys.

Interacting with your local keys requires you to install the Notary client. Binaries can be found at <https://github.com/docker/notary/releases>. Here is a quick script to add it your system.

    wget -O /usr/local/bin/notary https://github.com/docker/notary/releases/download/v0.4.3/notary-Linux-amd64
    chmod 755 /usr/local/bin/notary

At the same time, getting the notary client DTR's CA public key is also needed.

    mkdir -o ~/.docker/tls/dtr.example.com; curl -sk https://dtr.example.com/ca -o ~/.docker/tls/dtr.example.com/ca.crt

It is easy to simplify the notary command with an alias.

    alias notary="notary -s https://dtr.example.com -d ~/.docker/trust --tlscacert ~/.docker/tls/dtr.example.com/ca.crt"

With the alias in place, run `notary key list` to show the local keys and where they are stored.

    ROLE       GUN                          KEY ID                                                              LOCATION
    ----       ---                          ------                                                              --------
    root                                    44d193b5954facdb5f21584537774b9732cfea91e5d7531075822c58f979cc93    /root/.docker/trust/private
    targets    ...ullet.com/admin/alpine    2a0738c4f75e97d3a5bbd48d3e166da5f624ccb86899479ce2381d4e268834ee    /root/.docker/trust/private

To make the keys more secure it is recommended to always store the `root_keys` offline, meaning, not on the machine used to sign the images. If that machine were to get compromised then they would have everything needed to sign "bad" images. Yubikey is a really good method for storing keys offline.



##### Use a Yubikey

Notary can be used with a hardware token storage device called a [Yubikey](https://www.yubico.com/products/yubikey-hardware/). The Yubikey must be prioritized to store root keys and requires user touch-input for signing. This creates a two-factor authentication for signing images. Note that Yubikey support is included with the Docker Engine 1.11 client for use with Docker Content Trust. The specific use is to have all of your developers use Yubikeys with their workstations. You can get more information about Yubikeys from the [Docker docs](https://docs.docker.com/notary/advanced_usage/#/use-a-yubikey).



##### Signing with Jenkins

When teams get large it becomes harder to manage all the developer keys. One method for reducing the management load is to not let developers sign images. Using Jenkins to sign all the images that are destined for production eliminates most of the key management. The keys on the Jenkins server still need to be protected and backed up.

The first step is to create a user account for your CI system. For example, assume Jenkins is the CI system. As an admin user logged in to UCP, navigate to **User Management** and select **Add User**. Create a user with the name `jenkins` and set a strong password.

Next, create a team called `CI` and add the `jenkins` user to this team. The signing policy is team based, so the `jenkins` user must be in a team.

![](./images/ucp-ci-jenkins.jpg)

While still logged in as an admin, navigate to **Admin Settings** and select the **Content Trust** subsection. Select the checkbox to enable Content Trust and in the select box that appears, select the `CI` team that was just created. Save the settings.

This policy requires every image that is referenced in a `docker pull`, `docker run`, or `docker service create` must be signed by a key corresponding to a member of the `CI` team. In this case, the only member is the `jenkins` user.

![](./images/ucp-signing-policy.jpg)

The signing policy implementation uses the certificates issued in user client bundles to connect a signature to a user. Using an incognito browser window (or otherwise), log into the `jenkins` user account you created earlier. Download a client bundle for this user. It is also recommended that you change the description associated with the public key stored in UCP such that you can identify in the future which key is being used for signing.

Please note each time a user retrieves a new client bundle, a new keypair is generated. It is therefore necessary to keep track of a specific bundle that a user chooses to designate as the user's signing bundle.

Once you have decompressed the client bundle, the only two files you need for the purposes of signing are `cert.pem` and `key.pem`. These represent the public and private parts of the user’s signing identity respectively. Load the `key.pem` file onto the Jenkins servers, and use `cert.pem` to create delegations for the `jenkins` user in our Trusted Collection.

On the Jenkins server, use the notary client to load keys. Simply run `notary -d /path/to/.docker/trust key import /path/to/key.pem`. You will be asked to set a password to encrypt the key on disk. For automated signing, this password can be configured into the environment under the variable name `DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE`. The `-d` flag to the command specifies the path to the trust subdirectory within the server’s Docker configuration directory. Typically this is found at `~/.docker/trust`.

There are two ways to enable Content Trust: globally and per operation. To enabled Content Trust globally, set the environment variable `DOCKER_CONTENT_TRUST=1`. To enable on a per operation basis, wherever you run `docker push` in your Jenkins scripts, add the flag `--disable-content-trust=false`. You may wish to use this second option if you only want to sign some images.

The Jenkins server is now prepared to sign images, but you need to create delegations referencing the key to give it the necessary permissions.

Any commands displayed in this section should not be run from the Jenkins server. You will most likely want to run them from your local system.

If this is a new repository, create it in Docker Trusted Registry (DTR).

Next, initialize the trust data and create the delegation that provides the Jenkins key with permissions to sign content. The following commands initialize the trust data and rotate snapshotting responsibilities to the server. This is necessary to ensure human involvement it not required to publish new content.

Create an alias to streamline all of the following commands. The alias sets the server and the default trust store location. Adding the CA for DTR's TLS certificate is needed if the certificate is signed by a root server.

    alias notary="notary -s https://dtr.example.com -d ~/.docker/trust --tlscacert ~/.docker/tls/dtr.example.com/ca.crt"

Initialize the repository if you have not pushed a signed image:

    notary init dtr.example.com/admin/alpine
    notary key rotate dtr.example.com/admin/alpine snapshot -r
    notary publish dtr.example.com/admin/alpine

Now that the repository is initialized, create the delegations for Jenkins. Docker Content Trust treats a delegation role called targets/releases specially. It considers this delegation to contain the canonical list of published images for the repository. It is therefore generally desirable to add all users to this delegation with the following command:

    notary delegation add dtr.example.com/admin/alpine targets/releases --all-paths /path/to/cert.pem

This solves a number of prioritization problems that would result from needing to determine which delegation should ultimately be trusted for a specific image. However, because it is anticipated that any user will be able to sign the targets/releases role, it is not trusted in determining if a signing policy has been met. Therefore it is also necessary to create a delegation specifically for Jenkins:

    notary delegation add dtr.example.com/admin/alpine targets/jenkins --all-paths /path/to/cert.pem

Next publish both these updates (remember to add the correct `-s` and `-d` flags):

    notary publish dtr.example.com/admin/alpine

> Informational (Advanced): When including the targets/releases role in determining if a signing policy had been met, there is the potential of images being opportunistically deployed when an appropriate user signs it. In the scenario described so far, only images signed by the `CI` team (containing only the `jenkins` user) should be deployable. If a user “Moby” could also sign images but was not part of the `CI` team, he might sign and publish a new target/release that contained his image. UCP would refuse to deploy this image because it was not signed by the `CI` team. However, the next time Jenkins published an image, it would update and sign the targets/releases role as whole, enabling “Moby” to deploy his image.



##### Key Delegation

Similar to delegating a key for Jenkins, you can delegate multiple keys for teams. There are a few tricks for this. When adding a delegation, it is recommended to add the delegation for the targets/releases role as well as a role to indicate the team.

![](./images/key_delegation.jpg)

For example, with these three teams: developer, qa, and devops in the targets/releases role, it would be best to add each to the targets/releases role and also create a role for each key:

    #Keep in mind the '-p' is to automatically publish.
    # create delegation for the targets/releases role for each of the keys
    notary delegation add -p dtr.exampleexample.com/admin/alpine targets/releases --all-paths ~/developer.pem ~/qa.pem ~/devops.pem
    
    # create delegation to the targets/developer role
    notary delegation add -p dtr.example.com/admin/alpine targets/developer --all-paths ~/developer.pem
    
    # create delegation to the targets/qa role
    notary delegation add -p dtr.example.com/admin/alpine targets/qa --all-paths ~/qa.pem
    
    # create delegation to the targets/devops role
    notary delegation add -p dtr.example.com/admin/alpine targets/devops --all-paths ~/devops.pem

This is ideal so in the case of everyone pushing to the same tag, you end up with the same hash in targets/developer, targets/qa, and targets/devops, and then whoever signed last also signed the same hash into targets/releases. Without the signature on targets/releases, you can't pull the image and having individual roles for each, you get additional data about which signatures are actually in place based off of which key.



##### Lost key? Rotate it?

If you lost your root or signing key, all hope is not lost. The keys can simply be rotated. Then re-pushing the image will trigger a resigning.

To rotate the "targets" (signing) key:

    root @ ~  notary key rotate dtr.example.com/admin/alpine targets
    Enter passphrase for new targets key with ID 00aeaf3 (dtr.example.com/admin/alpine): 
    Repeat passphrase for new targets key with ID 00aeaf3 (dtr.example.com/admin/alpine): 
    Enter username: admin
    Enter password: 
    Enter passphrase for root key with ID 2a0738c: 
    Successfully rotated targets key for repository dtr.example.com/admin/alpine

Notice you have to enter a new passphrase for the target key. Also note that Notary removes the old targets key and replaces it with the new one. The behavior is a little different for the root key. Notary keeps the old root key to ensure your downstream clients can transition. Next, rotate the root key:

    root @ ~  notary key rotate dtr.example.com/admin/alpine root
    Warning: you are about to rotate your root key.
    
    You must use your old key to sign this root rotation. We recommend that
    you sign all your future root changes with this key as well, so that
    clients can have a smoother update process. Please do not delete
    this key after rotating.
    
    Are you sure you want to proceed?  (yes/no)  yes
    You are about to create a new root signing key passphrase. This passphrase
    will be used to protect the most sensitive key in your signing system. Please
    choose a long, complex passphrase and be careful to keep the password and the
    key file itself secure and backed up. It is highly recommended that you use a
    password manager to generate the passphrase and keep it safe. There will be no
    way to recover this key. You can find the key in your config directory.
    Enter passphrase for new root key with ID 75cb534: 
    Repeat passphrase for new root key with ID 75cb534: 
    Enter username: admin
    Enter password: 
    Successfully rotated root key for repository dtr.example.com/admin/alpine

Remember to keep the keys private, and when you can, use a hardware token like a Yubikey. Currently only the Yubikey version 4 is compatible.



#### Key Verification

There are some more useful notary commands to list and even unsign images:

    ### verify image is signed
    notary list dtr.example.com/admin/alpine -r targets/releases
    notary list dtr.example.com/admin/alpine -r targets/admin
    
    ### unsign image
    notary remove -p dtr.example.com/admin/alpine latest -r targets/releases
    notary remove -p dtr.example.com/admin/alpine latest -r targets/admin
    
    ### verify image is no longer signed
    notary list dtr.example.com/admin/alpine -r targets/releases
    notary list dtr.example.com/admin/alpine -r targets/admin



### Image Scanning

Starting with version 2.2.0, DTR includes on-premises image scanning. The on-prem scanning engine within DTR scans images against the [CVE Database](https://cve.mitre.org/). First, the scanner performs a binary scan on each layer of the image, identifies the software components in each layer, and indexes the SHA of each component. This binary scan evaluates the components on a bit-by-bit basis, so vulnerable components are discovered regardless of filename, whether or not they're included on a distribution manifest or in a package manager, whether they are statically or dynamically linked, or even if they are from the base image OS distribution.

The scan then compares the SHA of each component against the CVE database (a "dictionary" of known information security vulnerabilities). When the CVE database is updated, the scanning service reviews the indexed components for any that match newly discovered vulnerabilities. Most scans complete within a few minutes, however larger repositories may take longer to scan depending on your system resources. The scanning engine gives you a central point to scan all the images and delivers a Bill of Materials (BOM), which can be coupled with [Notary](#dtr-notary) to ensure an extremely secure supply chain for your images.

![](./images/secure_scan.png)



#### Setup Image Scanning

Before you begin, make sure that you or your organization has purchased a DTR license that includes Docker Security Scanning,and that your Docker ID can access and download this license from the Docker Store.

To enable Image Scanning, go to **Settings --> Security**, select **Enable Scanning**, and then select whether to use the Docker-supplied CVE database (**Online** — the default option) or use a locally-uploaded file (**Offline** — this option is only recommended for environments that are isolated from the Internet or otherwise can't connect to Docker for consistent updates). Once enabled in online mode, DTR downloads the CVE database from Docker, which may take a while for the initial sync. If your installation cannot access `[https://dss-cve-updates.docker.com/](https://dss-cve-updates.docker.com/ "https://dss-cve-updates.docker.com/")` you must manually upload a `.tar` file containing the security database.

* If you are using **Online** mode, the DTR instance contacts a Docker server, download the latest vulnerability database, and install it. Scanning can begin once this process completes.
* If you are using **Offline** mode, use the instructions in **Update scanning database - offline mode** to upload an initial security database.

![](./images/dtr-scan-setting.jpg)

By default, when Security Scanning is enabled, new repositories automatically scan on `docker push`, but any repositories that existed before scanning was enabled are set to "scan manually" mode by default. If these repositories are still in use, you can change this setting from each repository's **Settings** page.



#### CVE Offline Database

If your DTR instance cannot contact the update server, you can download and install a `.tar` file that contains the database updates. These offline CVE database files are handled through the [Docker Store](https://store.docker.com/).



#### Scanning Results

To see the results of the scans, navigate to the repository itself, then click **Images**. A clean image scan has a green checkmark shield icon:

![](./images/dtr-scan-clean.jpg)

A vulnerable image scan has a red warning shield:

![](./images/dtr-scan-vuln.jpg)

There are two views for the scanning results, **Layers** and **Components**. The **Layers** view shows which layer of the image had the vulnerable binary. This is extremely useful when diagnosing where the vulnerability is in the Dockerfile:

![](./images/dtr-scan-binary.jpg)

The vulnerable binary is displayed, along with all the other contents of the layer, when you click the layer itself. In this example there are two vulnerable binaries:

![](./images/dtr-scan-layer.jpg)

Click the vulnerable image to see the **Components** view. From the **Component** view the CVE number, a link to CVE database, file path, layers affected, severity, and description of severity are available:

![](./images/dtr-scan-component.jpg)

Now you can take action against and vulnerable binary/layer/image.

If you discover vulnerable components, check if there is an updated version available where the security vulnerability has been addressed. If necessary, contact the component's maintainers to ensure that the vulnerability is being addressed in a future version or patch update.

If the vulnerability is in a `base layer` (such as an operating system) you might not be able to correct the issue in the image. In this case, you might need to switch to a different version of the base layer, or you might find an equivalent, less vulnerable base layer. You might also decide that the vulnerability or exposure is acceptable.

Address vulnerabilities in your repositories by updating the images to use updated and corrected versions of vulnerable components, or by using a different components that provide the same functionality. When you have updated the source code, run a build to create a new image, tag the image, and push the updated image to your DTR instance. You can then re-scan the image to confirm that you have addressed the vulnerabilities.

What happens when there are new vulnerabilities released? There are actually two phases. The first phase is to fingerprint the image's binaries and layers into hashes. The second phase is to compare the hashes with the CVE database. The fingerprinting phase takes the longest amount of time to complete. Comparing the hashes is very quick. When there is a new CVE database, DTR simply compares the existing hashes with the new database. This process is also very quick. The scan results are always updated.



#### Webhooks

DTR includes webhooks for common events, such as pushing a new tag or deleting an image. This allows you to build complex CI and CD pipelines from your own DTR cluster. Currently all the webhook administration is handled through the API.

The webhook events you can subscribe to are as follows:

Repository specific events:

* Tag push
* Tag delete
* Manifest push
* Manifest delete
* Security scan completed
* Security scan failed

Namespace specific events:

* Repo events (created/updated/deleted)

Global events:

* Security scanner update complete

To subscribe to an event you need to be at least an admin of the particular repository (for repository events) or namespace (for namespace events). A global administrator can subscribe to any event. For example, a user must be an admin of repository to subscribe to its tag push events.

More information about webhooks can be found in the [Docker docs](https://docs.docker.com/datacenter/dtr/2.2/guides/webhooks/). DTR also presents the API by going to the menu under the login in the upper right, and then **API docs**.

![](./images/dtr-api.jpg)

In the API docs you can see all sorts of examples for extending DTR. Near the bottom is the webhooks section. Examples and even a "Try it out" function is available.

![](./images/dtr-webhooks.jpg)



## Summary

From limiting root access to nodes to using RBAC for UCP and DTR to storing secrets securely, this document provides all the security information you need to create a secure, customized, containerized infrastructure.

