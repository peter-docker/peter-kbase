---
type: reference
version: 1.0
title: "Docker Reference Architecture: Building a Docker Secure Supply Chain"
summary: "This reference architecture describes the components that make up a Secure Supply Chain. Topics include using Git, Jenkins, and the Docker Store to feed the supply chain. All the tools listed and demonstrated within this reference architecture can be replaced with alternatives."
dateCreated: "Wed, 11 Oct 2017 04:08:00 GMT"
dateModified: "Wed, 11 Oct 2017 04:08:00 GMT"
dateModified_unix: 1507694880
upvote: 0
internal: no
author: "clemenko"
platform:
  - linux
  - windows server
testedon:
  - ee-17.06.1-ee
  - dtr-2.3.0
  - ucp-2.2.0
tags:
  - security
product:
  - EE
---

<a name="introduction"></a>
## Introduction
Creating a Secure Supply Chain of images is vitally important. Every organization needs to weigh ALL options available and understand the security risks. Having so many options for images makes it difficult to pick the right ones. Ultimately every organization needs to know the provenance of all the images, even when trusting an upstream image from [store.docker.com](http://store.docker.com). Once the images are imported into the infrastructure, a vulnerability scan is vital. Docker Trusted Registry with Image Scanning gives insight into any vulnerabilities. Finally, everything needs to be automated to provide a succinct audit trail.

<a name="what-you-will-learn"></a>
## What You Will Learn
This reference architecture describes the components that make up a Secure Supply Chain. Topics include using Git, Jenkins, and [the Docker Store](http://store.docker.com) to feed the supply chain. All the tools listed and demonstrated within this reference architecture can be replaced with alternatives. The Secure Supply Chain can be broken into three stages:

- Stage 1 is a code repository.
- Stage 2 is Continuous Integration.
- Stage 3 is a registry that can scan images.

Even though there are many alternatives, this document focuses on one set:

- GitLab (Stage 1)
- Jenkins (Stage 2)
- Docker Trusted Registry (Stage 3)

One motto to remember for this reference architecture is "NO human will build or deploy code headed to production!"

<a name="prerequisites"></a>
## Prerequisites
Before continuing, become familiar with and understand:

- [Docker Concepts from the Docker docs](https://docs.docker.com/engine/understanding-docker/)
- [Securing Docker EE and Security Best Practices](https://success.docker.com/Architecture/Docker_Reference_Architecture%3A_Securing_Docker_EE_and_Security_Best_Practices)

<a name="abbreviations"></a>
## Abbreviations
The following abbreviations are used in this document:

* UCP = Universal Control Plane
* DTR = Docker Trusted Registry
* DCT = Docker Content Trust
* EE = Docker Enterprise Edition
* RBAC = Role Based Access Controls
* CA = Certificate Authority
* CI = Continuous Integration
* CD = Continuous Deployment
* HA = High Availability
* BOM = Bill of Materials
* CLI = Command Line Interface

<a name="why"></a>
## Why
There are several good reasons why you need a **Secure Supply Chain**. Creating a **Secure Supply Chain** is theoretically mandatory for production. Non-production pipelines can also take advantage of having an automated base image. When thinking about Supply Chain, a couple of key phrases come to mind:

* "NO human will build or deploy code headed to production!"
   * It helps prevent malicious code from being snuck into approved code. It also helps prevent insider threat.
* "Everything needs an audit trail."
   * Being able to prove the what, when, why, and how questions makes everyone's job easier.
* "Every supply chain needs a known good source."
   * Would you build a house in the middle of a swamp?

Ideally you want the shortest path for images. You want to guarantee the success of the image making it through the chain. Limiting the steps is a great way to reduce the number of moving parts. At a high level, only two components, _Git (GitLab) and Docker Trusted Registry (DTR)_, are necessary.

Below is a basic diagram of the path today.
![](./images/supply_chain_workflow.png)

<a name="begin"></a>
## Known Good Source
No matter how good a supply chain is, it all depends on starting with a "Known Good Source". Stage 1 can be broken down into two possible starting points.

- Automated, pre-built, verified, and preferably certified images from [the Docker Store](https://store.docker.com)
- A private, secure Git repository with Dockerfiles and other YAMLs

There are good reasons for both. The Docker Store path means that the upstream image is inherited with a bit of risk in how the vendor built it. The Git path means there are risks taken when building the image. Both entry points have their pros and cons. Both starting points have verifiable contents to ensure they are a "known good source".

The next sections look at both sources in more detail.

<a name="store"></a>
## Docker Store
The Docker Store, [store.docker.com](https://store.docker.com), should be the first place to look for images. It provides huge advantages for every organization. Store contains a vast quantity of images that are ready for use. The owners of the images carry the responsibility of updating and ensuring that there are no vulnerabilities. Thanks to Docker Store all "Certified" and "Official" images are scanned for vulnerabilities. Docker Store and Vendors take things a step further with "Certified" images. Certified images go through an [extensive vetting process](https://success.docker.com/Store). Certified images essentially come with a guarantee from the vendor and Docker that the container will work. Store also includes "Official" images.  "Official" images are built by Docker and also updated regularly. Docker Store and [Docker Hub](https://hub.docker.com) also contain community images. Community images should be used at last resort.

![](./images/store.docker.com.jpg)

<a name="upstream"></a>
### Picking the Right Upstream
Picking the right images from [Store](https://store.docker.com) is critical. The decision process for which image to use is simple. Start with [Certified Images](https://blog.docker.com/2017/03/announcing-docker-certified/), then move on to official images. Lastly, consider community images. Only use community images that are an "automated build." This helps ensure that they are updated in a timely fashion.  Verification of the freshness of the image is important as well.

From a blog post on [Certified Images](https://blog.docker.com/2017/03/announcing-docker-certified/):

*The Docker Certification Program is designed for both technology partners and enterprise customers to recognize Containers and Plugins that excel in quality, collaborative support and compliance. Docker Certification is aligned to the available Docker EE infrastructure and gives enterprises a trusted way to run more technology in containers with support from both Docker and the publisher. Customers can quickly identify the Certified Containers and Plugins with visible badges and be confident that they were built with best practices, tested to operate smoothly on Docker EE.*

![](./images/certified_program.jpg)

When searching [Docker Store](https://store.docker.com) for images, make sure to check the **Docker Certified** checkbox.

![](./images/store_certified.jpg)

One of the great features about [Docker Store](https://store.docker.com) is that the images are scanned for security vulnerabilities. This allows for inspection of the images before pulling.

![](./images/store_nginx.jpg)

Besides "Certified" images on [Store](https://store.docker.com) another great resource is Hub's ["Official" images](https://hub.docker.com/explore/). All these "Official" images are automated, scanned, and updated quite often.

When using upstream images that are not Official, or Certified ensure that the image is an "automated build". Reviewing the Dockerfile is an important step to ensure only the correct bits are in the image. Last resort is to consider creating an "automated" image for the community.

Please keep in mind that ANY image pulled from Hub or Store should also receive the same level of scrutiny through Docker Trusted Registry.

<a name="gitlab"></a>
## Git - (GitLab CE)
In today's modern enterprise: version control systems are the center of all code. Version control systems such as Git are also a great way to keep track of configuration. Git really becomes the "Source of Truth" for your enterprise. There are several companies that produce Git servers. [GitLab CE](https://store.docker.com/./images/gitlab-community-edition) is a great open source one. In the following example, GitLab Community Edition is used.

The ideal Git repo structure contains all the files necessary for building and deploying. Specifically the `Dockerfile`, any code, and the `stack.yml`. The `Dockerfile` is the build recipe for the Docker image. The code files should be obvious. The `stack.yml` is used for describing the stack. The `stack.yml` is also known as a compose YAML file. The following example sets up a GitLab instance with Docker. This should be outside your Docker Enterprise Edition cluster.

<a name="gitlab_setup"></a>
### Setup
GitLab has some good instructions for [setting up with Docker images](https://docs.gitlab.com/omnibus/docker/). Because Git uses the SSH port (22) by default, either the host's port or Git's port needs to be changed. The following shows how to move GitLab's port to 2022. For production, moving the host's SSH port might make more sense. Also, permanent storage is needed for a stateful install. Use a stack file to simplify the installation:

```
version: "3.3"
services:
  gitlab:
    image: gitlab/gitlab-ce:latest
    ports:
      - 80:80
      - 443:443
      - 2022:22
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /srv/gitlab/config:/etc/gitlab
      - /srv/gitlab/logs:/var/log/gitlab
      - /srv/gitlab/data:/var/opt/gitlab
    restart: always
    networks:
      gitlab:

  gitlab-runner:
    image: gitlab/gitlab-runner:alpine
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /srv/gitlab-runner/config:/etc/gitlab-runner
      - /root/.docker:/root/.docker
      - /root/.notary:/root/.notary
    restart: always
    networks:
      gitlab:

networks:
  gitlab:
```

Save this as `gitlab.yml`. Then execute the following commands:

```
$ sudo docker swarm init
$ sudo docker stack deploy -c gitlab.yml gitlab
```

Note that it will take a minute for GitLab to start.

The next section disucsses the repository contents.

<a name="repo_contents"></a>
### Repository Contents
One great way to leverage the "Source of Truth" is to store all the contents that compose the image and the stack together. This example is a three tier app made up of "web," "middleware," and "db". Storing all three Dockerfiles and `stack.yml` in the same location makes it incredible easy for all the teams to build and deploy. Ideally, the directory structure would contain:

1. Dockerfiles named `web.dockerfile`, `middleware.dockerfile`, and `db.dockerfile`.
2. Source bits and artifacts for each tier in a separate directory.
3. A `stack.yml`, which is used for `docker stack deploy`.
4. GitLab CI declarative, `.gitlab-ci.yml`, which is discussed later.

Don't forget to utilize multi-stage builds in Dockerfiles. Multi-stage builds help to greatly reduce the size of the resulting image. Now, you can automate everything.

![](./images/gitlab_dir_contents.jpg)

<a name="gitlab_ci"></a>
## Continuous Integration Automation
In order to leverage the idea of "No human will push code to production.", you need to automate all the things. Thanks to recent editions of GitLab, you can now configure CI/CD functions directly. There is no longer a need for additional tools. This greatly simplifies the setup and maintainability. To take advantage of the CI/CD, first register at least one runner. The runner is included in the `gitlab.yml` from the previous setup section. The next step is to configure the runner.

<a name="gitlab_config"></a>
### Configure the Runner
GitLab, similar to other CI tools, requires runners to complete the builds. To active the runner installed with the `docker stack deploy` from the previous section, you need to get the runner token. Navigate to **Admin Area** --> **Runners**. Here you can find the token needed to register the runner.

![](./images/gitlab_add_runner.jpg)

Luckily there is a shortcut to registering the runner. Simply ssh into the GitLab node and run the following Docker command (Notice the token from the GitLab CE page.):

```
docker exec -it $(docker ps --format '{{.Names}}\t{{.ID}}'|grep runner|awk '{print $2}') gitlab-runner \
register -n \
--url http://gitlab.example.com \
--registration-token <^>$token<^^> \
--executor docker \
--description "local docker" \
--docker-image "docker:latest" \
--docker-volumes "/var/run/docker.sock:/var/run/docker.sock" \
--docker-volumes "/root/.docker:/root/.docker"
```

Once registered you should see the Runner available with a `shared` tag. You also want to make sure that the `Run untagged jobs` is checked and that the `Lock to current projects` is unchecked.

<a name="gitlab-yml"></a>
### Build Declarative
Here is where the CI magic happens. GitLab looks to a file at the root of the repository called `.gitlab-ci.yml`. This is the CI declarative file.

> **Note:** Check out the [GitLab documentation](https://docs.gitlab.com/ee/ci/yaml/README.html) on this topic.

Consider the following scenarios. Be sure to configure the variables in each individual repository.

Here is a good example of building an image from git itself.

```
# Official docker image.
variables:
  DOCKER_DRIVER: overlay2

image: docker:latest

before_script:
  - docker login -u <^>$DTR_USERNAME<^^> -p <^>$DTR_PASSWORD<^^> <^>$DTR_SERVER<^^>

build:
  stage: build
  script:
    - docker build --pull -t dtr.example.com/admin/"<^>$CI_PROJECT_NAME<^^>"_build:<^>$CI_JOB_ID<^^> .
    - docker push dtr.example.com/admin/"<^>$CI_PROJECT_NAME<^^>"_build:<^>$CI_JOB_ID<^^>
    - docker rmi dtr.example.com/admin/"<^>$CI_PROJECT_NAME<^^>"_build:<^>$CI_JOB_ID<^^>
```

Here is a good example of a `.gitlab-ci.yml` for pulling, tagging, and pushing an image from store.docker.com to your DTR.

```
# Official docker image.
variables:
  DOCKER_DRIVER: overlay2

image: docker:latest

before_script:
  - docker login -u <^>$DTR_USERNAME<^^> -p <^>$DTR_PASSWORD<^^> <^>$DTR_SERVER<^^>

stages:
  - signer

signer:
  stage: signer
  script:
    - docker pull <^>$DTR_SERVER<^^>/admin/flask:latest
    - export DOCKER_CONTENT_TRUST=1
    - docker push <^>$DTR_SERVER<^^>/admin/flask:latest
    - docker rmi <^>$DTR_SERVER<^^>/admin/flask:latest
```

![](./images/gitlab_add_variable.jpg)

GitLab is now setup with variables and build declaratives. Next, add a trigger to your project.

<a name="gitlab-triggers"></a>
### Pipeline Triggers
GitLab now includes an awesome CI tool as well as a way to trigger the pipeline remotely. GitLab calls these triggers. The easiest way to create these triggers is to navigate to **Projects** --> **Settings** --> **CI/CD** --> **Pipeline triggers** --> **Expand**.

![](./images/gitlab_triggers.jpg)

Here is an example of the trigger format. `http://gitlab.example.com/api/v4/projects/<^><PROJECT><^^>/trigger/pipeline?token=<^><TOKEN><^^>&ref=<^><REF><^^>`. The three fields that are needed here are `<^><PROJECT><^^>`, `<^><TOKEN><^^>`, and `<^><REF><^^>`. `<^><REF><^^>` should be set to the branch name. `<^><TOKEN><^^>` should be set to the token you get from GitLab. The best way to get the `<^><PROJECT><^^>` is to simply copy the URL in the **Pipeline Triggers** page. You will need the Pipeline Triggers later.

Next, add Docker Trusted Registry.

<a name="dtr"></a>
## Docker Trusted Registry
Docker Trusted Registry is much more than a simple registry. DTR now includes some great features that increase the strength of the supply chain. Some of the new features include image promotion and immutability.

The following sections look at some of these new features.

<a name="dtr-scan"></a>
### Image Scanning
Starting with version 2.2.0, DTR includes on-premises image scanning. The on-prem scanning engine within DTR scans images against the [CVE Database](https://cve.mitre.org/). First, the scanner performs a binary scan on each layer of the image, identifies the software components in each layer and indexes the SHA of each component. This binary scan evaluates the components on a bit-by-bit basis, so vulnerable components are discovered regardless of filename, whether or not they're included on a distribution manifest or in a package manager, whether they are statically or dynamically linked, or even if they are from the base image OS distribution.

The scan then compares the SHA of each component against the CVE database (a "dictionary" of known information security vulnerabilities). When the CVE database is updated, the scanning service reviews the indexed components for any that match newly discovered vulnerabilities. Most scans complete within a few minutes, however larger repositories may take longer to scan depending on your system resources. The scanning engine gives you a central point to scan all the images and delivers a Bill of Materials (BOM), which can be coupled with [Notary](#dtr-notary) to ensure an extremely secure supply chain for your images.

Starting with DTR 2.3.0, the scanning engine can also scan Windows binaries.

![](./images/secure_scan.png)

<a name="dtr-scan-setup"></a>
#### Setup Image Scanning
Before you begin, make sure that you or your organization has purchased a DTR license that includes Docker Security Scanning and that your Docker ID can access and download this license from the Docker Store.

By default, when Security Scanning is enabled, new repositories automatically scan on `docker push`, but any repositories that existed before scanning was enabled are set to "scan manually" mode by default. If these repositories are still in use, you can change this setting from each repository's **Settings** page.

To enable Image Scanning, go to **Settings --> Security**, select **Enable Scanning**, and then select whether to use the Docker-supplied CVE database (**Online** — the default option) or use a locally-uploaded file (**Offline** — this option is only recommended for environments that are isolated from the Internet or otherwise can't connect to Docker for consistent updates).

Once enabled in online mode, DTR downloads the CVE database from Docker, which may take a while for the initial sync. If your installation cannot access `https://dss-cve-updates.docker.com/` you must manually upload a `.tar` file containing the security database.

* If you are using **Online** mode, the DTR instance contacts a Docker server, download the latest vulnerability database, and install it. Scanning can begin once this process completes.
* If you are using **Offline** mode, use the instructions in **Update scanning database - offline mode** to upload an initial security database.
![](./images/dtr-scan-setting.jpg)

<a name="dtr-scan-offline"></a>
#### CVE Offline Database
If your DTR instance cannot contact the update server, you can download and install a `.tar` file that contains the database updates. These offline CVE database files can be retrieved from [Store.docker.com](https://store.docker.com) under **My Content** --> License **Setup**.

![](./images/store-license.jpg)

<a name="dtr-scan-results"></a>
#### Scanning Results
To see the results of the scans, navigate to the repository itself, then click **Images**. A clean image scan has a green checkmark shield icon:

![](./images/dtr-scan-clean.jpg)

A vulnerable image scan has a red warning shield:

![](./images/dtr-scan-vuln.jpg)

There are two views for the scanning results, **Layers** and **Components**. The **Layers** view shows which layer of the image had the vulnerable binary. This is extremely useful when diagnosing where the vulnerability is in the Dockerfile:

![](./images/dtr-scan-binary.jpg)

The vulnerable binary is displayed, along with all the other contents of the layer, when you click the layer itself. In this example there are a few potentially vulnerable binaries:

![](./images/dtr-scan-layer.jpg)

Click the vulnerable image to see the **Components** view. From the **Component** view the CVE number, a link to CVE database, file path, layers affected, severity, and description of severity are available:

![](./images/dtr-scan-component.jpg)

Now you can take action against and vulnerable binary/layer/image.

If you discover vulnerable components, check if there is an updated version available where the security vulnerability has been addressed. If necessary, contact the component's maintainers to ensure that the vulnerability is being addressed in a future version or patch update.

If the vulnerability is in a `base layer` (such as an operating system) you might not be able to correct the issue in the image. In this case, you might need to switch to a different version of the base layer, or you might find an equivalent, less vulnerable base layer. You might also decide that the vulnerability or exposure is acceptable.

Address vulnerabilities in your repositories by using updated and corrected versions of vulnerable components or by using different components that provide the same functionality. When you have updated the source code, run a build to create a new image, tag the image, and push the updated image to your DTR instance. You can then re-scan the image to confirm that you have addressed the vulnerabilities.

What happens when there are new vulnerabilities released? There are actually two phases. The first phase is to fingerprint the image's binaries and layers into hashes. The second phase is to compare the hashes with the CVE database. The fingerprinting phase takes the longest amount of time to complete. Comparing the hashes is very quick. When there is a new CVE database, DTR simply compares the existing hashes with the new database. This process is also very quick. The scan results are always updated.

Now that you have scan results, it is time to add a Promotion Policy.

<a name="dtr-promotion"></a>
### Image Promotion Policy
The release of Docker Trusted Registry 2.3.0 added a new way to promote images. You can create policies for promotion based upon thresholds for vulnerabilities, tag matching, and package names, and even the license. This gives great powers in automating the flow of images. It also ensures that images that don't match the policy don't make it to production. The criteria are as follows:

* Tag Name
* Package Name
* All Vulnerabilities
* Critical Vulnerabilities
* Major Vulnerabilities
* Minor Vulnerabilities
* All Licenses

![](./images/dtr-promotion.jpg)

You can create and view the policies from either the source or the target. The following is an example of **All Vulnerabilities**. It sets up a promotion policy for the `admin/flask_build` repo to "promote" to `admin/flask` if there are zero vulnerabilities.

First, navigate to the source repository, and go to the **Policies** tab. From there select **New Promotion Policy**. Select the **All Vulnerabilities** on the left. Then click **less than or equals**, enter `0` (zero) into the textbox, and click **Add**. Now select a target for the promotion. On the right hand side, select the namespace and image to be the target. Click **Save & Apply**.  Applying the policy executes against the source repository. **Save** applies the policy to future pushes.

Notice the **Tag Name In Target**. This option provides the ability to change the tag according to some variables. It is recommended that you start out leaving the tag name the same. For more information please check out the [Image Promotion Policy docs](https://docs.docker.com/datacenter/dtr/2.3/guides/user/create-promotion-policies/).

![](./images/dtr_promo_policy.jpg)

Notice the **PROMOTED** badge. One thing to note is that the Notary signature is not promoted with the image. This means a CI system must sign the promoted images. This can be achieved with the use of webhooks and promotion policy.

![](./images/dtr_promoted.jpg)

Consider a DTR setup where the base images get pushed from Jenkins to DTR. Then the images get scanned and promoted if they have zero vulnerabilities — part of a good **Secure Supply Chain**. This leads to Image Immutability.

<a name="dtr-immutability"></a>
### Image Immutability
DTR 2.3.0 and higher has the option to set a repository to **Immutable**. Setting a repository to **Immutable** means the tags can not be overwritten. This is a great feature for ensuring that your base images do not change over time. This example is of the Alpine base image. Ideally CI would update the base image and push to DTR with a specific tag. Being **Immutable** simply guarantees that you can always go back to the specific tag and trust it has not changed. This can be extended with an Image Promotion Policy.

![](./images/dtr-immutable.jpg)

<a name="dtr-immutability_policy"></a>
### Image Immutability + Promotion Policy
A great example of using the Promotion Policy with Immutable tags is when you are building images directly from Git. This example uses a simple flask app. The `.gitlab-ci.yml` has three basic steps : build, push, remove. Push to a DTR repository, `dtr.example.com/admin/flask_build`, where Immutability is turned on, into a private repository.

![](./images/dtr_pub_repos.jpg)

GitLab pushes with the build number as the tag. The format looks like : `dtr.example.com/admin/flask_build:66`. Since Immutability is turned on, the tag of `66` can never be overwritten. This gives a solid foundation. Next add two promotion policies based on the same thresholds. The first policy promotes the image to a public repo `dtr.example.com/admin/flask` with the same tag. The second policy promotes with the `latest` tag.

![](./images/dtr_flask_policies.jpg)

The result is that the image gets "promoted" twice. Once with the same tag and once with the `latest` tag.

![](./images/dtr_promoted.jpg)

The next link in the chain is to have webhooks.

<a name="dtr-webhooks"></a>
### Webhooks
Starting with DTR 2.3.0, webhooks can be managed through the GUI. DTR includes webhooks for common events such as pushing a new tag or deleting an image. This allows you to build complex CI and CD pipelines from your own DTR cluster.
The webhook events you can subscribe to are as follows (repository specific):

* Tag push
* Tag delete
* Manifest push
* Manifest delete
* Security scan completed
* Security scan failed
* Image promoted from repository

Webhooks are created on a per-repository basis. More information about webhooks can be found in the [Docker docs](https://docs.docker.com/datacenter/dtr/2.3/guides/user/create-and-manage-webhooks/). DTR also presents the API by navigating to the menu under the login in the upper right, and then selecting **API docs**.

This example is a continuation of the previous example that uses the `dtr.example.com/admin/flask_build` repository. Now, add a webhook. To add one using the "Image promoted from repository" event, the webhook must ben configured to tell GitLab to use Notary and sign the image.

![](./images/dtr-webhook.jpg)

For reference the `WEBHOOK URL` we used is`http://gitlab.example.com/api/v4/projects/<^><PROJECT><^^>/trigger/pipeline?token=<^><TOKEN><^^>&ref=<^><REF><^^>`. The three fields that are needed here are <^><PROJECT><^^>, <^><TOKEN><^^>, and <^><REF><^^>.  <^><REF><^^> should be set to the branch name. <^><TOKEN><^^> should be set to the token you get from GitLab. <^><PROJECT><^^> can be obtained from the trigger creation page. More details about triggers can be found in the [Triggers](#gitlab-triggers) section. The great thing about the webhooks and triggers is they can kick off new jobs, similar to image signing.

<a name="dtr-notary"></a>
### Content Trust/Image Signing — Notary

*Notary is a tool for publishing and managing trusted collections of content. Publishers can digitally sign collection, and consumers can verify integrity and origin of content. This ability is built on a straightforward key management and signing interface to create signed collections and configure trusted publishers.*

Docker Content Trust/Notary provides a cryptographic signature for each image. The signature provides security so that the image you want is the image you get. If you are curious about what makes Notary secure, read about [Notary's Architecture](https://docs.docker.com/notary/service_architecture/). Since Docker EE is "Secure by Default," Docker Trusted Registry comes with the Notary server out of the box.

A successfully signed image has a green check mark in the DTR GUI.

![](./images/dtr-signed.jpg)

<a name="gitlab_sign"></a>
##### Signing with GitLab
When teams get large it becomes harder to manage all the developer keys. One method for reducing the management load is to not let developers sign images. Using GitLab to sign all the images that are destined for production eliminates most of the key management. The keys on the GitLab server still need to be protected and backed up.

The first step is to create a user account for your CI system. For example, assume GitLab is the CI system. Navigate to DTR's web interface. As an admin user, navigate to **Organizations**, and select **New organization**. Call this new organization `ci`. Now add a Jenkins user by navigating into the organization, and selecting **Add User**. Create a user with the name `gitlab`, and set a strong password. This creates a new user and adds them to the `ci` organization. Next give the GitLab user `Org Admin` status so that the user can manage the repositories under the `ci` organization. Next, navigate to UCP's **User Management**. Create a team under the `ci` organization called `gitlab`.

![](./images/dtr_add_user.jpg)

Now that the team is setup, turn on the policy enforcement. Navigate to **Admin Settings**, and select the **Docker Content Trust** subsection. Select the **Run Only Signed Images** checkbox to enable Docker Content Trust. In the select box that appears, select the `ci` team that was just created. Save the settings.

This policy requires every image that is referenced in a `docker pull`, `docker run`, or `docker service create` to be signed by a key corresponding to a member of the `gitlab` team. In this case, the only member is the `gitlab` user.

![](./images/ucp-signing-policy.jpg)

The signing policy implementation uses the certificates issued in user client bundles to connect a signature to a user. Using an incognito browser window (or otherwise), log into the `gitlab` user account you created earlier. Download a client bundle for this user. It is also recommended that you change the description associated with the public key stored in UCP such that you can identify in the future which key is being used for signing.

Please note each time a user retrieves a new client bundle, a new keypair is generated. It is therefore necessary to keep track of a specific bundle that a user chooses to designate as the user's signing bundle.

Once you have decompressed the client bundle, the only two files you need for the purposes of signing are `cert.pem` and `key.pem`. These represent the public and private parts of the user’s signing identity respectively. Load the `key.pem` file onto the Jenkins servers, and use `cert.pem` to create delegations for the `gitlab` user in our Trusted Collection.

On the GitLab server, populate the local `/root/.docker/` directory, and initialize the notary repository. On the `gitlab` server, do the following:

```
#become root
sudo -i

#change to the /root directory
cd /root

#make directory for CA
mkdir -p /root/.docker/tls/dtr.example.com/

#copy CA to the notary/docker location
curl -sk https://dtr.example.com/ca -o /root/.docker/tls/dtr.example.com/ca.crt

#get the notary client
wget https://github.com/docker/notary/releases/download/v0.4.3/notary-Linux-amd64

#chmod it
chmod 755 notary

#setup alias
alias notary="notary -s https://dtr.example.com --tlscacert /root/.docker/tls/dtr.example.com/ca.crt -d ~/.docker/trust"

#check notary
./notary key list

#init the repository. Make all the passwords the same to simplify things.
notary init -p dtr.example.com/admin/flask

# rotate snapshot key
notary key rotate dtr.example.com/admin/flask -r snapshot

# setup releases role
notary delegation add dtr.example.com/admin/flask targets/releases cert.pem --all-paths

# setup user role
notary delegation add dtr.example.com/admin/flask targets/gitlab cert.pem --all-paths

# publish changes
notary publish dtr.example.com/admin/flask
```

To enable the automated signing, the variable `DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE` needs to be configured within the GitLab project similar to the Build Declarative variables. Next, create the GitLab project for signing. Create a new project with the following `.gitlab-ci.yml`. This uses the local `/root/.docker/trust` directory.

```
# Official docker image.
variables:
  DOCKER_DRIVER: overlay2

image: docker:latest

before_script:
  - docker login -u <^>$DTR_USERNAME<^^> -p <^>$DTR_PASSWORD<^^> <^>$DTR_SERVER<^^>

stages:
  - signer

signer:
  stage: signer
  script:
    - docker pull <^>$DTR_SERVER<^^>/admin/flask:latest
    - export DOCKER_CONTENT_TRUST=1
    - docker push <^>$DTR_SERVER<^^>/admin/flask:latest
    - docker rmi <^>$DTR_SERVER<^^>/admin/flask:latest
```

Here is what a successful output from the signing project should look like:

![](./images/gitlab_completed_signing.jpg)

Now the final steps is to create a Pipeline Trigger for the Image Signing project. Use the webhook with an "Image promoted from repository" event.


<a name="conclusion"></a>
## Conclusion
Automating a **Secure Supply Chain** is not that difficult. After following this reference architecture, GitLab is setup with at least two projects. One is for the code, Dockerfile, and stack yaml. A second is for the image signing component. DTR also has two repositories. One is for the private build, and a second is for the signed promoted image.

The main goal is to have an image that is both **Promoted**, based on a good scan,and **Signed**, with Notary, in an **automated** fashion.

![](./images/dtr_signed_promoted.jpg)

While specific tools were discussed, there are a few takeaways from this reference architecture:

* Automate all the things
* Pick a Known Good Source
* Leverage Image scanning and promotion
* Sign the Image
* NO human will build or deploy code headed to production!

Here is another look at the workflow.

![](./images/supply_chain_workflow.png)

Consider the ideas and feel free to change the individual tools out for what your organization has.
