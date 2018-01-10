---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Group membership queries in .NET fail when using gMSA
internal: no             # set to yes to keep it internal-only
comment: ""
type: kbase               # set to customerservice if applicable
author:  tfoxnc
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)

testedon:
platform:     
  - windows server
tags:      
  - error
  - installing
---
## Issue

When using gMSA on a Windows Server node, the group membership method `UserPrincipal.GetAuthorizationGroups()` fails with the following error:

```
System.DirectoryServices.AccountManagement.PrincipalOperationException: While trying to retrieve the authorization groups, an error (1789) occurred.
```

## Resolution

Group Managed Service Accounts (gMSA) require that the whole domain be upgraded to 2012 Active Directory Schema. The two reasons for that are:

- The schema update tells older domain controllers about the gMSA object type which is only available on 2012 schema.
- gMSA requires the use of a Key Distribution service that will be hosted on the 2012 domain controller. The Key Distribution Service is what actually serves up the current gMSA passwords to the hosts. Since, hosts will need to talk to the Key Distribution Service to obtain passwords it is recommended to have at least a couple of domain controllers at 2012 Active directory level for high availability.


