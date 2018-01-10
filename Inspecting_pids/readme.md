---
title: Mapping containers processes to namespaces for troubleshooting
internal: yes 
type: guide
author:  aathomas
product: 
  - EE
  - CE
platform:
  - linux 
testedon:
  - ee-17.06.2-ee-4
tags: 
  - troubleshooting
---
### How to inspect specific container pids and map them to network namespaces

| **video:**  | *overview* |
| :---:            | :--- |
| ![](https://img.youtube.com/vi/XQE8t6raS00/3.jpg)](https://www.youtube.com/watch_popup?v=XQE8t6raS00) | Quick overview of how to map process ID's (PIDS) to netns namespaces in /var/run/netns to help isolate processes and their behaviors. **Possible additional security validations**

Mapping containers processes to namespaces for troubleshooting and possibly to confirm isolation concerning nested child namespace around security and specifically target networking issues in container namespaces directly on their associated Linux node's. This method is only dependent on coreutils functionality.

Manual breakdown of process isolation similar to the nsenter <sup>1</sup> command.

    # /usr/bin/nsenter -m -t $(pid dockerd) nsenter --net=$(docker inspect --format {{.NetworkSettings.SandboxKey}} $(docker ps -qlf name=swarm))

## Prerequisites

Before performing these steps, you must meet the following requirements:

- A working docker-ee cluster of anyform or standalone docker-engine built on CentOS or RHEL Linux. In this example CentOS 7.4 is utilized. 

## Process


1. Create the following dir 
   
   ```
   # mkdir -p /var/run/netns/
   ```

2. Change into the `/var/run/netns/`
  
    ```
    # cd /var/run/netns/
    ```

3. Obtain the pid you want to inspect using `# docker ps` then inspect the process or container with `# docker top` in the example below we will target swarm on a UCP manager node. 

      ```
      # docker ps|grep swarm|awk '{print $1}'|xargs docker top
      ```
4. Create a symlink to the pid in the namespace directory where $PID is the process ID from the last step.  

    ```
    # ln -sf /proc/$PID/ns/net "swarm"
    ``` 
5. Profit with netns isolation. See the video examples above for more usage. 

    ```
    # ip a netns exec swarm <related commands>
    ```

## What's Next

Additional information relating to security validation.

### **Footnotes**
| *note*       | *info*        |
| :---:        |     :---      |
| 1 | *After speaking with trapier++ concerning feedback around this kbase he relayed that the `nsenter` command referenced sourced on this kbase is doing the samething as the manually steps 1-5. I will isolate any differences and update the documentation with additional information.* |
