---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Promote a member of an organization to Organization Admin with the UCP API
type: kbase
author:  adamancini
product:
  - EE
platform:
testedon: 
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.4
tags: 
  - API
---

## Issue

Sometimes it may be necessary to promote users programmatically via the command line.

## Resolution

The following API call promotes a member of a team/organization to an Admin user:
```
curl -kL -H "Content-Type: application/json" --user <^><admin-username><^^> -X PUT https://localhost:443/enzi/v0/accounts/<^><team><^^>/members/<^><user_to_promote><^^> -d '{"isAdmin":true}'
```
> **Note:**  `--user ${admin}` must refer to an existing user with Administrator privileges

For example, to promote the user `alice`:
```
curl -vvvkL -H "Content-Type: application/json" --user <^>admin<^^> -X PUT https://localhost:443/enzi/v0/accounts/<^>teamxps<^^>/members/<^alice<^^> -d '{"isAdmin":true}'
```
> **Note:** The above example uses curl's `-v` flag for extra verbosity.
