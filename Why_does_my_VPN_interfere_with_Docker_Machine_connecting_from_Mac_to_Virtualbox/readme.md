---
type: kbase
version: 5
title: "Why does my VPN interfere with Docker Machine connecting from Mac to Virtualbox?"
summary: "If your VPN software modifies your routes and forces all network traffic through the VPN, you might have connectivity issues to all local networks (including the Virtualbox host­-only network). The Virtualbox process on the host will simply start listening on localhost and then forward the packets into the NAT mode network for the VM. For example, set up a forward that has the Host IP set to 127.0.0.1, Host Port set to 80, Guest Host set to '' (empty), and Guest Port set to 80."
dateCreated: "Mon, 19 Dec 2016 15:27:01 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "scasey"
platform:
  - mac
testedon:
tags:
product:
---

Some VPN software prevents access to local network resources. Docker­ Machine setup creates a Virtualbox host-­only network, which is essentially a "local network." If your VPN software modifies your routes and forces all network traffic through the VPN, you might have connectivity issues to all local networks (including the Virtualbox host­-only network).

One workaround is to use NAT for the Virtualbox network, which doesn't disable traffic to localhost. The Virtualbox process on the host will simply start listening on localhost and then forward the packets into the NAT mode network for the VM. To modify Virtualbox to use NAT:

1. Open the Virtualbox GUI.
2. Choose your Virtualbox Docker­ Machine VM.
3. Right click and choose **Settings**.
4. Choose the **Network** tab.
5. Select **Adapter 1** (which should show as attached to NAT).
6. Under **Advanced**, choose **Port Forwarding**.
7. Under **Port Forwarding**, create port forwards between your host and your Virtualbox's eth0 interface.

For example, set up a forward that has the **Host IP** set to 127.0.0.1, **Host Port** set to 80, **Guest Host** set to "" (empty), and **Guest Port** set to 80. Now, you should be able to get into the VM by connecting to the virtualbox host's 127.0.0.1 on port 80, dropping into VM's eth0 port 80.