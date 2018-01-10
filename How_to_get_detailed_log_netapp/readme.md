---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to collect detailed logs from NetApp plugin
internal: yes 
comment: "These commands were suggested by NetApp support while working on support case 00028371"
type: kbase             
product:       
  - ee        
testedon:         
  - ee-17.03.2-ee-6
  - ucp-2.1.5
platform:           
  - linux
tags:               
  - daemon
  - logging
---
## Issue
 
For some plugin issues, it is clear if the root cause is in a Docker product or plugin.  When that happens, Docker Support works with the corresponding vendor through TSANet collect detailed log for both support teams.  

This article describes how to get detailed logs from the NetApp plugin.  However, similar steps should be applicable to other plugins.
  
## Prerequisites

Before performing these steps, you must meet the following requirements:
 - Determine if the issue is obviously caused by Docker EE components or purely a plugin application issue (in which case customer must contact vendor directly) 
 - Make sure the plugin comes from a TSANet partner, usually a Docker certified container image on Docker Store

## Resolution

NetApp plugin (and possibly many others) can be restarted in debug mode with following steps:
 
1. Stop the plugin.
    ```
    $ docker plugin disable netapp 
    ```

2. Enable debug mode.
    ```
    $ docker plugin set netapp debug=true 
    ```

3. Restart plugin.
    ```
    $ docker plugin enable netapp 
    ```

4. Reproduce the issue.

5. Collect the log output.
    ```
    $ journalctl -u docker 
    ```

6. Stop the plugin again, disable debug mode, and restart it.


## What's Next

Internal process documentation for TSANet is found on [Confluence]( 
https://docker.atlassian.net/wiki/spaces/DSS/pages/132668073/TSANet+Support+Process)
