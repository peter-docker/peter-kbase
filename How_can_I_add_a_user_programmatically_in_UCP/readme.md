---
type: kbase
version: 4
title: "Add a user to UCP using the API on the command line"
summary: "You can add a user programmatically in UCP by using the UCP API along with the curl command. After retrieving the UUID token, you can add a user this way: curl -k -H 'Authorization: Bearer {auth-token}' -X POST https://ucp-url/api/accounts -d '{'username':'username', 'password':'password', 'first_name':'firstname', 'role':1, 'admin':true}' Note: The UCP API is experimental and is subject to change in future versions, possibly affecting backwards compatibility."
dateCreated: "Thu, 18 Aug 2016 09:22:32 GMT"
dateModified: "2018-01-09T11:09:48-06:00"
dateModified_unix: 1515517788
upvote: 0
internal: no
author: yiming
platform:
testedon:
  - ucp-1.1.2
tags:
product:
  - EE
---

**THIS IS THE REALLY COOL EDIT I WANTED TO ADD**

You can add a user programmatically in UCP by using the UCP API along with the `curl` command.

First, get a UUID token for authentication:

    curl -k -s -H "Content-Type: application/json" -X POST -d '{"username": "<^>admin-username<^^>", "password": "<^>admin-password<^^>"}' 'https://ucp-url/auth/login'

After retrieving the UUID token, you can add a user this way:

    curl -k -H "Authorization: Bearer {auth-token}" -X POST https://ucp-url/api/accounts -d '{"username":"<^>username<^^>", "password":"<^>password<^^>", "first_name":"<^>firstname<^^>", "role":<^>1<^^>, "admin":<^>true<^^>}'

For the role parameter, use one of the following values:

| Number | Value |
|---|---|
| 0 | No Access |
| 1 | View Only |
| 2 | Restricted Control |
| 3 | Full Control |

Note: The UCP API is experimental and is subject to change in future versions, possibly affecting backwards compatibility.
