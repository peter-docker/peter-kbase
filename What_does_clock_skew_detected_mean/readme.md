---
type: kbase
version: 2
title: "What does clock skew detected mean?"
summary: "UCP requires that the system clocks on all the machines in a UCP cluster be in sync or else it can start having issues checking the status of the different nodes in the cluster. To ensure that the clocks in a cluster are synced, you can use NTP to set each machine's clock. This will install NTP on each machine and start ntpd, which is a daemon which periodically syncs your machine's clock with a central server."
dateCreated: "Fri, 11 Nov 2016 13:13:07 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "justinnevill"
platform:
  - linux
testedon:
tags:
product:
  - EE
---

UCP requires that the system clocks on all the machines in a UCP cluster be in sync or else it can start having issues checking the status of the different nodes in the cluster. To ensure that the clocks in a cluster are synced, you can use NTP to set each machine's clock.

First, on each machine in the cluster, install NTP. For example, to install NTP on an Ubuntu distribution, run:

    sudo apt-get update && apt-get install ntp

On CentOS and RHEL, run:

    sudo yum install ntp

On SUSE Linux, run:

    sudo zypper ref && zypper install ntp

This will install NTP on each machine and start ntpd, which is a daemon which periodically syncs your machine's clock with a central server. To sync the clocks right away, on each machine run:

    sudo ntpdate pool.ntp.org

You can check to see if your machine's time is in sync with its NTP servers by running:

    sudo ntpq -p

The output will look similar to the following:

    remote           refid      st t when poll reach   delay   offset  jitter
    ==============================================================================
     45.35.50.61     139.78.97.128    2 u   24   64    1   60.391  4623378   0.004
     time-a.timefreq .ACTS.           1 u   23   64    1   51.849  4623377   0.004
     helium.constant 128.59.0.245     2 u   22   64    1   71.946  4623379   0.004
     tock.usshc.com  .GPS.            1 u   21   64    1   59.576  4623379   0.004
     golem.canonical 17.253.34.253    2 u   20   64    1  145.356  4623378   0.004

This will show how much your machine's clock is out of sync with its NTP servers.
