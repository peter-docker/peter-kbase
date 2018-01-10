---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Migrating from UCP 2.1 Access Controls to UCP 2.2 Collections
type: kbase
author:  adamancini
product: 
  - EE
platform:
testedon: 
  - ee-17.06.2-ee-3
  - ucp-2.2.0
tags:
  - access control
  - security
  - upgrading
---

## Issue

In UCP 2.1, the settings under **Admin Settings** are set to prevent users from scheduling tasks on UCP manager nodes. After upgrading to UCP 2.2, these settings are not working anymore to prevent users from scheduling on any node.

## Prerequisites
Before performing these steps, you must meet the following requirements:

- You've just upgraded from a UCP environment < 2.2
- You have previously configured Users & Teams and Access Labels to implement access controls for your resources

## Details

In UCP 2.2, by default, everyone in the `docker-datacenter` organization has scheduler access to the top-level collection, Swarm (otherwise shown as `/`). RBAC controls in the new UCP 2.2 system are configured in a heirarchy like a directory tree. Permissions applied to a certain collection apply to everything below that. By default, a collection is made for the organization Org - `docker-datacenter` that everyone belongs to, which allows the scheduler access to `/`. The default collections `/Shared` and `/System` both fall under the root `/`, so the scheduler grant on `/` allows anyone to schedule containers on any nodes.

To prevent users from scheduling on UCP manager nodes, at a minimum, go to the **Grants** section under **User Management** and remove the default Org - `docker-datacenter` grant.

To fully implement node-based RBAC, first create some collections underneath `/`. When you click the **Collections** tab, you should see `/System` and `/Shared`. The `/Shared` collection is where worker nodes are placed by default. UCP/DTR nodes should be left in `/System`.

By default, everyone has access to their private collection under `/Shared/private/<username>`. The default access level for their private collections can be set in the **Authentication & Authorization** section under **Admin Settings**.

Set up two collections, `/Shared/production` and `/Shared/development`. Then, move some worker nodes into `/Shared/production` and `/Shared/development` by clicking on the **Nodes** tab to the left, then clicking the node to be moved, and selecting **Configure** from the menu at the right, then **Collections**. Then go to **User Management** > **Teams & Organizations** and create 2 teams under the `docker-datacenter` organization: `prod_team` and `dev_team`.

If you already have users in the system, click on the team, and in the menu at the right, select **Actions** > **Add Users**. Check the boxes by the users to add to the team and click **Add Users** at the bottom.

With teams populated with users, you can create Grants based on those teams to give access to specific collections. A collection can be any resource, nodes, secrets, images, networks, etc. Click on **User Management** > **Grants**, and **Create Grant**. On the **Collections** tab, drill down to the Collection you want to grant access to (**Swarm** > **View Children** > **System** > **View Children** > **production** > **Select Collection**), then click the **Roles** tab and select an appropriate Role for this grant. Finally, click **Subjects** and assign this grant to an organization/team or a single user.

Note that the RBAC controls can only restrict people who are logged into UCP with a client bundle or through the UI. If someone logs directly into a manager node and uses `docker run` the container will still run on that node.

## What's Next

Additional resources:

- [Access Control in UCP 2.2](https://docs.docker.com/datacenter/ucp/2.2/guides/access-control/)
- [Node-Specifc Access Control](https://docs.docker.com/datacenter/ucp/2.2/guides/access-control/access-control-node/)
