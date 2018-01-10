---
type: kbase
version: 3
title: "Post upgrade UPC 2.1.0 to UCP 2.1.1 all users are unable to log into UCP and DTR"
summary: "After upgrading from UPC 2.1.0 to UCP 2.1.1, you might receive the following error: resulting in all users not being able to log into UCP and DTR. The command will prompt for the admin user’s password and then return the current sessions config, similar to: If perUserLimit is set to 0, use the following command to change it to a value between 1 and 100. Be sure to use the same values for lifetimeHours and renewalThresholdHours as the ones found in step 1: 3.Try to log into UCP and DTR."
dateCreated: "Tue, 28 Mar 2017 12:15:18 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
testedon:
  - ucp-2.1.0
  - ucp-2.1.1
tags:
  - upgrading
  - error
product:
  - EE
---

After upgrading from UPC 2.1.0 to UCP 2.1.1, you might receive the following error:

`API Errors: ["INVALID_AUTHENTICATION_CREDENTIALS: invalid authentication credentials given - Detail: invalid session"]`

resulting in all users not being able to log into UCP and DTR.

Resolve this issue by performing the following steps:

1. Start by getting the current configuration for user sessions by running the following command:

    curl -u admin "https://<^>$UCP_HOST<^^>/enzi/v0/config/sessions"

The command will prompt for the `admin` user’s password and then return the current sessions config, similar to:

    {
      "lifetimeHours": 72,
      "renewalThresholdHours": 24,
      "perUserLimit": 0
    }

2. If `perUserLimit` is set to 0, use the following command to change it to a value between 1 and 100. The value of 5 is recommended. Be sure to use the same values for `lifetimeHours` and `renewalThresholdHours` as the ones found in step 1:

    curl -u admin "https://$UCP_HOST/enzi/v0/config/sessions" \ -X PUT \ -H 'Content-Type: application/json' \ -d '{"lifetimeHours": <^>72<^^>, "renewalThresholdHours": <^>24<^^>, "perUserLimit": <^>5<^^>}'

3.Try to log into UCP and DTR. You should be able to successfully log into them now.
