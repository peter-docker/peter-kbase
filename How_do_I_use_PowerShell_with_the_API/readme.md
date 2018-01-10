---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How do I use PowerShell to interact with the V2 API?
internal: no
comment:
type: kbase
author:  stevenfollis
product:
  - ee
testedon:
  - ucp-2.2.4
platform:
  - windows
  - windows server
tags:
  - API
---

## Issue

Microsoft [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/powershell-scripting?view=powershell-5.1) can be used with the Docker Universal Control Plane (UCP) API to automate common administrative tasks associated with a Docker Enterprise Edition cluster.

## Generating an Authorization Token

All requests made against the UCP API require an authorization token. To generate a token, generate a POST message against the `/auth/login` endpoint with an account's username and password enclosed in the body as JSON.

In this example, the token is retrieved and stored as the `$AUTH_TOKEN` variable:

```PowerShell
$USERNAME=""
$PASSWORD=""
$UCP_FQDN=""

# Retrieve Authentication Token
$AUTH_TOKEN = (Invoke-RestMethod `
    -Uri "https://$UCP_FQDN/auth/login" `
    -Body "{`"username`":`"$USERNAME`",`"password`":`"$PASSWORD`"}" `
    -Method POST).auth_token
```

Generate a new token at a regular interval, as tokens expire after a set duration.

## Common Tasks

Once a token is generated, it can be used to query various UCP API endpoints:

```PowerShell
#==============================================
# Get all nodes within the cluster
#==============================================
$NODES = Invoke-RestMethod `
    -Uri "https://$UCP_FQDN/nodes" `
    -Method GET `
    -Headers @{"Accept"="application/json";"Authorization"="Bearer $AUTH_TOKEN"}



#==============================================
# Get User Accounts
#==============================================
$ACCOUNTS = Invoke-RestMethod `
    -Uri "https://$UCP_FQDN/accounts/" `
    -Method GET `
    -Headers @{"Accept"="application/json";"Authorization"="Bearer $AUTH_TOKEN"}



#==============================================
# Create a new User Account
#==============================================
$ACCOUNT = '{
    "name": "gford",
    "password": "password!",
    "fullName": "Garth Ford",
    "isOrg": false,
    "isAdmin": false,
    "isActive": true,
    "isImported": false
}'

Invoke-RestMethod `
    -Uri "https://$UCP_FQDN/accounts/" `
    -Method POST `
    -Body $ACCOUNT `
    -ContentType "application/json" `
    -Headers @{"Accept"="application/json";"Authorization"="Bearer $AUTH_TOKEN"} 



#==============================================
# Update a User Account
#==============================================
$UPDATED_ACCOUNT = '{
    "fullName": "Garth William Ford"
}'

Invoke-RestMethod `
    -Uri "https://$UCP_FQDN/accounts/gford" `
    -Method PATCH `
    -Body $UPDATED_ACCOUNT `
    -ContentType "application/json" `
    -Headers @{"Accept"="application/json";"Authorization"="Bearer $AUTH_TOKEN"} 



#==============================================
# Delete a User Account
#==============================================
Invoke-RestMethod `
    -Method DELETE "https://$UCP_FQDN/accounts/gford" `
    -Headers @{"Accept"="application/json";"Authorization"="Bearer $AUTH_TOKEN"}
```

Please see the [UCP API Documentation](https://docs.docker.com/datacenter/ucp/2.2/reference/api/) for detailed information for each route.
