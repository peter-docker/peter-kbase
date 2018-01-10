---
type: kbase
version: 3
title: "Error retreiving usage metrics observed in UCP UI"
summary: "The following usage metrics errors can be observed from the UCP UI: Error retrieving cpu usage metrics Error retrieving memory usage metrics Error retrieving disk usage metrics Running the docker logs command against the ucp-metrics container reveals the following error: Remove the metrics container and trigger the ucp-reconciler to run and reconcile the metrics container again:"
dateCreated: "Tue, 11 Jul 2017 15:26:49 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "squizzi"
platform:
testedon:
tags:
  - error
product:
  - EE
---

## Issue

* The following usage metrics errors can be observed from the UCP UI:

    Error retrieving cpu usage metrics
    Error retrieving memory usage metrics
    Error retrieving disk usage metrics

![](./images/screenshot_metrics.png)

* The `ucp-metrics` container does not start and appears to be `Exited` in `docker ps -a` output:

    $ docker ps -a --filter name=ucp-metrics
    CONTAINER ID        IMAGE                      COMMAND                  CREATED             STATUS                         PORTS               NAMES
    bfafbfc679e4        docker/ucp-metrics:2.1.4   "/bin/entrypoint.s..."   4 days ago          Exited (1) About an hour ago                       ucp-metrics

* Running the `docker logs` command against the `ucp-metrics` container reveals the following error:

    $ docker logs -t ucp-metrics 
    2017-07-10T23:43:59.173335762Z time="2017-07-10T23:43:59Z" level=error msg="Error opening memory series storage: leveldb/storage: corrupted or incomplete meta file" source="main.go:181"

# Resolution

1. Clear the corrupted metrics database using the commands below. All historical metrics will unfortunately be lost in this procedure:

    ```
    # docker run --rm -it -v ucp-metrics-data:/data alpine sh
    # rm -rf /data/* 
    CTRL+C
    ```
2. Remove the metrics container and trigger the ucp-reconciler to run and reconcile the metrics container again:

    ```
    # docker rm ucp-metrics -f
    # docker start ucp-reconcile 
    # docker logs -f ucp-reconcile
    ```

Once the reconciliation completes, metrics should be functional on this UCP node once again.
