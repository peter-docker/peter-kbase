---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How do I gather heap usage information for the Docker Engine?
internal: no            
comment: ""
type: kbase            
author:  bryceryan-docker
product: 
  - ee        
testedon:    
  - ee-17.06.2-ee-5
platform: 
  - linux
tags:        
  - daemon
  - monitoring
---
## Issue

Docker Technical Support has requested I gather or monitor the heap or memory utilization for Docker Engine on 
one or more of my nodes. How can I gather engine heap utilization information?

## Prerequisites

Before performing these steps, you must meet the following requirements:

- The **socat** package or program must be installed on the node(s) where you were requested to gather heap/memory data

## Resolution

To gather heap/memory utilization data on the Docker Engine, do the following on each node where it was requested:

1.  Place the Docker daemon in [debug mode](https://success.docker.com/article/How_do_I_enable_'debug'_logging_of_the_Docker_daemon).
2.  Use `socat` to expose the Docker daemon socket `/var/run/docker.sock` via TCP on the loopback interface.

    ```$ sudo socat -d -d TCP-LISTEN:8080,fork,bind=localhost UNIX:/var/run/docker.sock```

3.  Collect memory heap information from the engine.

    ```
    $ docker run --rm --net host -v $PWD:/root/pprof/ golang go tool pprof --svg --alloc_space localhost:8080/debug/pprof/heap
    $ docker run --rm --net host -v $PWD:/root/pprof/ golang go tool pprof --svg --inuse_space localhost:8080/debug/pprof/heap
    ```

4. Provide the following files to Docker support for further analysis.

    - `pprof.localhost:8080.alloc_objects.alloc_space.001.pb.gz`
    - `pprof.localhost:8080.inuse_objects.inuse_space.001.pb.gz`



