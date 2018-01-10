---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: ucp-agent-win container fails to start
internal: no             
comment: "raised during pre-sales engagement https://github.com/docker/orca/issues/9071"
type: kbase               
author:  eiichik
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
  - ee-17.06.2-ee-5
  - ucp-2.2.3
  - dtr-2.3.4
platform:           
  - linux
  - windows server
tags:               
  - error
  - installing
  - storage
  - upgrading
---
## Issue

UCP-agent-win container fails to start with following error message. 

```
fatal task error [module=node/agent/taskmanager node.id=pbb... task.id=d5gi3d... service.id=le77tl... error=starting container failed: 
container ad42fb49b7... encountered an error during CreateContainer: failure in a Windows system call: 
This application does not support the current operation on symbolic links. (0x5b8) extra info: 
...
```
```
Create container failed with error: container ad42fb49b7... encountered an error during CreateContainer: failure in a Windows system call: This application does not support the current operation on symbolic links. (0x5b8) extra info: 
...
```

## Prerequisites

Before performing these steps, you must meet the following requirements:

- UCP cluster is version 2.2 or above
- Docker EE engine is version 17.06 or above
- The rest of the UCP cluster with Linux managers and workers are functioning correctly

## Resolution

The error messages above may indicate issues on volumes.  The following steps will clean up the Windows node and rejoin them to the UCP cluster.
 
1. From the Windows worker node, remove the Windows nodes from the swarm using Powershell:

    ```
   PS> docker swarm leave
    ```
2. From the UCP Manager node, remove the ucp-agent-win service. 

    ```
   $ docker service rm ucp-agent-win
    ```
     _Removing the service will cause it to restart after 5 seconds._
3. On the Windows worker node, pull ucp-agent-win containers that match your UCP version.

    ```
    PS> docker image pull docker/ucp-agent-win:<^><UCP_version><^^>
    PS> docker image pull docker/ucp-dsinfo-win:<^><UCP_version><^^>
    ```
4. Delete the `C:/programData/docker/ucp` directory if it exists. 
5. Run the setup script to generate new UCP certificates.

    ```
    PS> docker container run --rm docker/ucp-agent-win:<^><UCP_version><^^> windows-script | powershell -noprofile -noninteractive -command 'Invoke-Expression -Command $input'
    ```
 6. Go back to Manager node, and get a join token for worker.
    ```
    $ docker swarm join-token worker
    ```
    > Alternatively, you can get join token from UCP Admin UI.  Go to the **Nodes** section, and click **Add Node** button on right side. 
 7. On the Windows workers, execute the `join` command from the previous step.

