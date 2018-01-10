---
type: kbase
version: 3
title: "Downgrading your Docker EE license"
summary: "In a standard tier Docker EE environment, worker nodes must be placed in the /Shared collection, and all manager nodes as well as worker nodes running DTR must be placed in the /System collection. If the node is not in the desired collection, move it to a different collection by changing the label value using docker node update --label-add com.docker.ucp.access.label=/Shared <hostname>"
dateCreated: "Wed, 16 Aug 2017 00:14:23 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "tfoxnc"
platform:
testedon:
  - ee-17.06.1-ee
  - dtr-2.3.0
  - ucp-2.2.0
tags:
product:
  - EE
---

As of Docker EE 17.06, several features are enabled by having an advanced-tier license in your system, such as a trial license. These features are the following:

On UCP:

* Moving worker nodes to collections other than `/Shared`
* Setting Label Constraints on collections

On DTR:

* Using image promotion policies
* Performing new image scans

However, you will not be able to downgrade your license from advanced to standard if you are actively using any of the advanced UCP features. In order to downgrade your license, you need to manually reconfigure UCP so that no advanced tier features are in use. To do that, use the instructions in the following sections before applying your new license.

# Move all nodes to their standard collections

In a standard tier Docker EE environment, worker nodes must be placed in the `/Shared` collection, and all manager nodes as well as worker nodes running DTR must be placed in the `/System` collection. If one or more of your nodes are in a different collection, you will not be able to apply your new license.

Moving nodes through the UI:

* On the **Nodes** page, select one of your nodes. You can verify that the node is not in `/Shared` or `/System` by looking at the **Collection** tab.

* Select **Configure** and then **Collection**. Select the appropriate collection for the node: 
    * If it's a manager node or a DTR replica, place the node under `/System` .
    * Otherwise, place it under `/Shared`.

Moving nodes through the CLI:

* Inspect a node via `docker node inspect <hostname>`, followed by the node's hostname.
* Look at the value of the `com.docker.ucp.access.label` label, which indicates the collection that this node is placed in. Ignore the labels starting with `com.docker.ucp.collection`, as they are managed by UCP automatically.
* If the node is not in the desired collection, move it to a different collection by changing the label value using `docker node update --label-add com.docker.ucp.access.label=/Shared <hostname>`

# Remove Label Constraints from all collections

In a standard tier Docker EE environment, collections cannot have any label constraints specified. To apply a standard tier license, you must manually remove all label constraints from existing collections.

Removing label constraints through the UI:

* On the **Collections** page, select a collection and verify whether it has any label constraints by looking at the **Label Constraints** tab.
* Select **Configure** and then **Label Constraints**.
* For all label constraints of that collection, press the **X** button to remove them and then select **Update** to confirm your selection.