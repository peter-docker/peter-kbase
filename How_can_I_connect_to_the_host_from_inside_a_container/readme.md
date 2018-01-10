---
type: kbase
version: 5
title: "How can I connect to the host from inside a container?"
summary: "To connect to the host system from inside a container, setup a bridge network with a static IP address accessible from within the container. It will appear in the list of your interfaces on the left in the Network panel. Enter an IP that will not conflict with any network you might connect to into the IP Address field. This works because as your traffic exits the container, the normal OSX routing rules are applied, and that IP address is locally available."
dateCreated: "Thu, 30 Jun 2016 19:42:36 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "programmerq"
platform:
  - mac
testedon:
tags:
product:
  - EE
---

To connect to the host system from inside a container, setup a bridge network with a static IP address accessible from within the container.

For example, if your host system is Mac OS X:

1. Go into **Network** under **System Preferences**.
2. Click on the gear icon and choose **Manage Virtual Interfaces**.
3. Click on the **+** button and choose **New Bridge**.
4. Give it a name and do *NOT* include any other interfaces on your system.
5. Click **Create**. It will appear in the list of your interfaces on the left in the **Network** panel.
6. Click on it, and change **Configure IPv4** from **Using DHCP** to **Manually**.
7. Enter an IP that will not conflict with any network you might connect to into the **IP Address** field. Click **Apply**.

This works because as your traffic exits the container, the normal OSX routing rules are applied, and that IP address is locally available.