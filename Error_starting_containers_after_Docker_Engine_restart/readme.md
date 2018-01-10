---
type: kbase
version: 3
title: "Error starting containers after Docker Engine restart"
summary: "Oct 6 23:30:50 ip-10-6-34-38 docker: time='2016-10-06T23:30:50.464448934-04:00' level=error msg='failed to receive event from containerd: rpc error: code = 13 desc = 'transport is closing' Oct 6 23:31:59 ip-10-6-34-38 docker: time='2016-10-06T23:31:59.747679472-04:00' level=error msg='failed to receive event from containerd: rpc error: code = 13 desc = 'transport is closing'"
dateCreated: "Fri, 28 Oct 2016 16:26:07 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "pvnovarese"
platform:
  - linux
testedon:
  - cse-1.11.1-cs1
  - cse-1.11.2-cs5
tags:
  - error
  - daemon
product:
  - EE
---

## Issue

When launching some containers with the `restart always` setting, the containers don't come back up with a restart of the Docker service. Manually start doesn't work either. It comes back up when the Docker service is restarted a second time.

This is in the `daemon.log`:

`Oct 6 23:26:21 ip-10-6-34-38 docker: time="2016-10-06T23:26:21.649221669-04:00" level=error msg="Force shutdown daemon"  

Oct 6 23:28:21 ip-10-6-34-38 docker: time="2016-10-06T23:28:21.526995237-04:00" level=error msg="Handler for POST /v1.23/containers/d60110edbc34/start returned error: rpc error: code = 6 desc = \"mkdir /run/containerd/d60110edbc342330f3c05ed2b10f523bb5a83ab80e1ef60835f07dca35fff47f: file exists\""  

Oct 6 23:30:50 ip-10-6-34-38 docker: time="2016-10-06T23:30:50.464448934-04:00" level=error msg="failed to receive event from containerd: rpc error: code = 13 desc = \"transport is closing\""  

Oct 6 23:30:53 ip-10-6-34-38 docker: time="2016-10-06T23:30:53-04:00" level=error msg="containerd: get exit status" error="containerd: process has not exited" id=d60110edbc342330f3c05ed2b10f523bb5a83ab80e1ef60835f07dca35fff47f pid=init systemPid=11450  

Oct 6 23:31:59 ip-10-6-34-38 docker: time="2016-10-06T23:31:59.747679472-04:00" level=error msg="failed to receive event from containerd: rpc error: code = 13 desc = \"transport is closing\""  

Oct 6 23:34:15 ip-10-6-34-38 docker: time="2016-10-06T23:34:15.454499564-04:00" level=error msg="failed to receive event from containerd: rpc error: code = 13 desc = \"transport is closing\""  

Oct 6 23:34:45 ip-10-6-34-38 docker: time="2016-10-06T23:34:45.829946171-04:00" level=error msg="failed to receive event from containerd: rpc error: code = 13 desc = \"transport is closing\""  

Oct 6 23:43:51 ip-10-6-34-38 docker: time="2016-10-06T23:43:51.200796857-04:00" level=error msg="failed to receive event from containerd: rpc error: code = 13 desc = \"transport is closing\""  

Oct 6 23:44:06 ip-10-6-34-38 docker: time="2016-10-06T23:44:06.197911042-04:00" level=error msg="Force shutdown daemon"  

Oct 6 23:45:53 ip-10-6-34-38 docker: time="2016-10-06T23:45:53.678123569-04:00" level=error msg="Failed to start container f16597aa8b860f594a347df1025aae4fe5d43fe0ec689650889d27c226019758: rpc error: code = 6 desc = \"mkdir /run/containerd/f16597aa8b860f594a347df1025aae4fe5d43fe0ec689650889d27c226019758: file exists\""  

Oct 6 23:45:53 ip-10-6-34-38 docker: time="2016-10-06T23:45:53.691295094-04:00" level=error msg="Failed to start container e9b034ae7d8b5816143830ca4cfcb90213d42c3a8db7ad16b9fd23665f986d61: rpc error: code = 6 desc = \"mkdir /run/containerd/e9b034ae7d8b5816143830ca4cfcb90213d42c3a8db7ad16b9fd23665f986d61: file exists\""  

Oct 6 23:45:53 ip-10-6-34-38 docker: time="2016-10-06T23:45:53.967624263-04:00" level=error msg="Failed to start container 0e595729871a9d1a0cf15651da33e96653e3de5f85884f2f40bc1d024042a245: rpc error: code = 6 desc = \"mkdir /run/containerd/0e595729871a9d1a0cf15651da33e96653e3de5f85884f2f40bc1d024042a245: file exists\""`  


## Prerequisites

* Docker CS Engine 1.11.x
* RHEL or other systemd-based host

## Resolution

In your systemd `docker.conf`file, add this line to the `[Service]` section:  
  

`# kill only the docker process, not all processes in the cgroup  

KillMode=process`  


This is added by default in Docker CS Engine 1.12.

Reference: [https://github.com/docker/docker/issues/25246](https://github.com/docker/docker/issues/25246 "https://github.com/docker/docker/issues/25246")
