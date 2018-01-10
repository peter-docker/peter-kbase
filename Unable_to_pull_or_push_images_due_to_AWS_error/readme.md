---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Unable to pull or push images due to AWS error
internal: no             
comment: ""
type: kbase               
author:  lenesto.page
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
  - 17.06.2-ee-3
  - dtr-2.3.3
platform:           
  - linux
tags:               
  - error
  - aws
---
## Issue

When using Docker via AWS instances, you can encounter an issue where you are unable to push or pull images because of the AWS Access keys:

```
Error response from daemon: manifest for dtr-qa-*** not found. This error is usually consistent with misconfigured AWS Access keys. This article will show how to overcome this error and DTR inconsistent state.
```

## Resolution

To successfully recover/re-enable pushing and pulling of images, use the following steps:

1. Review the `dtr-registry` container logs and search for an error consistent with the following:

    ```
    2017-11-29T21:52:25.270228280Z {"auth.user.name":"","err.code":"manifest unknown","err.detail":"s3aws: InvalidAccessKeyId: The AWS Access Key Id you provided does not exist in our records.\n\tstatus code: 403, request id: ***","err.message":"manifest 
    ```
2. Rotate your AWS Access keys to successfully pull and push images again using the instructions at  [https://aws.amazon.com/blogs/security/how-to-rotate-access-keys-for-iam-users/](https://aws.amazon.com/blogs/security/how-to-rotate-access-keys-for-iam-users/)
