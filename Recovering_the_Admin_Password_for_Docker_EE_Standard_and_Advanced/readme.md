---
type: guide
version: 5
title: "Recovering the Admin Password for Docker EE Standard and Advanced"
summary: "Use the db-addr in the running process to specify the same value to the enzi command to change the admin password: At the prompts, enter the username of the admin you wish to reset the password for, the new password, and confirm the new password: After successfully connecting to the UCP controller node, to look at all accounts within eNZi, connect to the eNZi database and lookup the contents of the accounts table to show only selected fields using the pluck function:"
dateCreated: "Thu, 16 Feb 2017 04:31:21 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "anoop.kumar"
platform:
testedon:
  - cse-1.13.1-cs1
  - ee-17.03.0-ee-1
tags:
product:
  - EE
---

The AuthN API or eNZi (as it is known internally and pronounced N-Z) is a centralized authentication and authorization service and framework for Docker EE Standard and Advanced. This API is completely integrated and configured into Docker EE and works seamlessly with UCP as well as DTR. This is the component and service under the hood that manages accounts, teams and organizations, user sessions, permissions and access control through labels, Single-Sign-On (Web SSO) through OpenID Connect, and synchronization of account details from an external LDAP-based system into Docker EE Standard and Advanced.

For regular day-to-day activities, users and operators need not be concerned with the AuthN API and how it works, however its features can be leveraged to automate many common functions and/or bypass the UCP UI altogether to manage and manipulate the data directly.

The RESTFul AuthN API endpoints are documented in the [eNZi API](https://docs.docker.com/apidocs/v2.0.1/#!/accounts/func1).

Interaction with eNZi can be accomplished in two ways: via the exposed RESTful eNZi API over HTTP or via the `enzi` command.

This document demonstrates how to use the `enzi` command to solve a common issue — forgetting the admin password and consequently unable to login as an admin. Such a situation would not allow the invocation of the eNZi RESTful service as well. As such it would be necessary to access the container running the `enzi` service using the `enzi` command to reset the admin user's password.

This is especially helpful when using LDAP Auth mode with only one recovery admin account configured.

## Reset the Admin Password

The first option is to attempt a reset of the admin password.

The `passwd` command on `enzi` requires the value for `--db-addr` pointing to the RethinkDB host and port. This information can be found in the process table on the eNZi container. First, connect to the controller node:

    docker exec -it ucp-auth-api sh

At the prompt for the controller node, use the following command to view the `enzi` processes:

    ps -eaf | grep enzi

You will see output similar to the following:

    1 nobody     0:01 /usr/local/bin/enzi --debug --db-addr=10.1.1.250:12383 --jsonlog api --root-prefix=enzi
    22 nobody     0:00 grep enzi

Use the `db-addr` in the running process to specify the same value to the `enzi` command to change the admin password:

    enzi --db-addr=10.1.1.250:12383 passwd --interactive

At the prompts, enter the username of the admin you wish to reset the password for, the new password, and confirm the new password:

    Admin Username: tesla
    Admin Password:
    Confirm Admin Password:

If successful, you will see the following message:

    INFO[0021] successfully set user account password: tesla

To run everything described as one command:

    docker exec -it ucp-auth-api enzi "$(docker inspect --format '{{ index .Args 0 }}' ucp-auth-api)" passwd --interactive

## Create Another Admin Account

An alternative method of addressing the issue of forgotten admin password is to create another admin user with a known password and then using this newly created admin to repair the issue as shown below.

Again, connect to the UCP controller:

    docker exec -it ucp-auth-api sh

Then execute the following `enzi` command with the proper `db-addr` value:

    enzi --debug --db-addr=10.1.1.250:12383 create-admin --interactive

Create a new admin account using the prompts:

    Admin Username: repair-admin
    Admin Password:
    Confirm Admin Password:

If sucessful, you will see the following:

    connecting to db ...
    DEBU[0019] connecting to DB Addrs: [10.1.1.250:12383]
    creating admin user account...
    successfully created admin user account: repair-admin

Now it should be possible to login with the newly created `repair-admin` user and password..

> Note: Accounts created manually (like in the process above) when the Auth mode is set to `LDAP` will get disabled during the subsequent sync cycle.

## Convert Non-Admin Account to an Admin Account

This option requires gaining access to the eNZi database running on `rethinkdb`. Assuming there are other regular accounts in the database, this option involves the conversion of a regular non-admin account into an admin account and then using the credentials of this account to log in.

The `rethinkdb` instance bundled with eNZi is hardened such that it does not provide direct HTTP or console access. A possible option is to use another Docker container running a nodejs driver as a RethinkDB client, attached to the same network as the `eNZi` database, and connected to the driver port of `12383`. Standard `ReQL` commands can be run using this setup in the same way traditional databases use SQL statements. The entire process is simplified using a baked image `anoop/reticule` that acts as a RethinkDB driver based on nodejs.

As before, this too needs to be run on a UCP Controller node. Connect to the UCP controller node using the `anoop/reticule` image:

    docker run -it -v ucp-auth-store-certs:/tls --net container:ucp-auth-store --rm anoop/reticule <your_ip_address> 12383

The following DepreciationWarning can be safely ignored:

    (node:1) DeprecationWarning: Using Buffer without `new` will soon stop working. Use `new Buffer()`, or preferably `Buffer.from()`, `Buffer.allocUnsafe()` or `Buffer.alloc()` instead.
        undefined

After successfully connecting to the UCP controller node, to look at all accounts within eNZi, connect to the eNZi database and lookup the contents of the `accounts` table to show only selected fields using the `pluck` function:

    r.db('enzi').table('accounts').pluck('name','isAdmin','isOrg','isActive')

The output will be similar to the following:

    [ { isActive: false,
        isAdmin: false,
        isOrg: true,
        name: 'docker-datacenter' },
      { isActive: true, isAdmin: true, isOrg: false, name: 'admin' },
      { isActive: true, isAdmin: false, isOrg: false, name: 'docker' },

This output shows an organization called `docker-datacenter`, which is not useful to log into UCP, and two users — one admin account: `admin` with `isAdmin: true`, the password for which is forgotten; and one non-admin account: `docker`, the password for which is intact. To reset the forgotten admin password, use the non-admin account by escalating its permissions to become an admin account using the following command:

    r.db('enzi').table('accounts').get(‘docker’).update({isAdmin: “true”})

Now, logging into UCP as `docker` reflects admin access, which can be used to reset or enable the admin account's access.

## Create Another Admin User Using RethinkDB

Finally, another option for recovering the lost admin password is to create an admin user from scratch.

Once again, collect to the UCP controller using the `anoop/reticule` image:

    docker run -it -v ucp-auth-store-certs:/tls --net container:ucp-auth-store --rm anoop/reticule <your_ip_address> 12383

Then execute the following to create the new admin user `backupadmin`:

    r.db('enzi').table('accounts').insert({
    ... name: 'backupadmin',
    ... id: 'backupadmin',
    ... isOrg: false,
    ... isAdmin: true,
    ... isActive: true,
    ... passwordHash: 'bcrypt$$2a$10$HEISWxBRRIJr26U0cJMwAePMP1vHO1GNbBVSPdg2zftONl/dVletW'})
    { deleted: 0,
      errors: 0,
      inserted: 1,
      replaced: 0,
      skipped: 0,
      unchanged: 0 }

The UCP admin console should now be accessible using `backupadmin`/`password` as the new admin credentials.

> Note: Accounts created manually (like in the process above) when the Auth mode is set to `LDAP` will get disabled during the subsequent sync cycle.