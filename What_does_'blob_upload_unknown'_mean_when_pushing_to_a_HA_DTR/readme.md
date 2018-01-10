---
type: kbase
version: 3
title: "What does 'blob upload unknown' mean when pushing to a HA DTR?"
summary: "The error blob upload unknown typically means an attempt was made to push a Docker image to an HA DTR installation, where the backend storage has not been configured for replication. If NFS mounts for all DTR replicas have been configured and the error persists, it may be because of the NFS server side settings do not meeting the requirements for NFS when in use with the registry."
dateCreated: "Thu, 18 May 2017 16:37:51 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
testedon:
tags:
product:
  - EE
---

The error `blob upload unknown` typically means an attempt was made to push a Docker image to an HA DTR installation, where the backend storage has not been configured for replication. This is typically an oversight during installation where the local filesystem is being used for storage but something like NFS isn't backing the storage. Switching to a replicated storage option solves this.

If NFS mounts for all DTR replicas have been configured and the error persists, it may be because of the NFS server side settings do not meeting the requirements for NFS when in use with the registry. The required NFS server options are `sync` and `actimeo=0`. These ensure the writes occur from one replica and are immediately available on others.

If you are using the AWS Quickstart Template, an S3 bucket has already been created but requires configuring DTR to use it:

* In DTR, go to **Settings** > St**orage**. Change the storage to **S3**. Set the **AWS region name** and **S3 bucket name**. The S3 bucket is already created. You can find this in the CloudFormation Stack's **Outputs** tab. The Key is `S3Bucket` , and the IAM role should already be setup.

* The **Access Key** and **Secret Key **get passed automatically. The root directory can be left blank to use the default root.

* After configuring DTR with S3, the ability to push and pull images is enabled, with them being stored in S3.