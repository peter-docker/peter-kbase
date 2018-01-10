---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Container exits with "non-zero exit (137)"
internal: no             # set to yes to keep it internal-only
comment: "Inspired by SF 30422"
type: kbase               # set to customerservice if applicable
author:  quaddo
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.06.2-ee-4
  - ucp-2.1.5
platform:           # Optional. Keep all that apply.
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - daemon
  - error
---
## Issue

If a container is no longer running, use the following command to find the status of the container:

```
docker container ls -a
```

This article explains possible reasons for the following exit code:

```
"task: non-zero exit (137)"
```

With exit code 137, you might also notice a status of `Shutdown` or the following failed message:

```
Failed 42 hours ago
```

## Resolution

The `"task: non-zero exit (137)"` message is effectively the result of a `kill -9` (`128 + 9`). This can be due to a couple possibilities (seen most often with Java applications):

1. The container received a `docker stop`, and the application didn't gracefully handle `SIGTERM` (`kill -15`) — whenever a `SIGTERM` has been issued, the docker daemon waits 10 seconds then issue a `SIGKILL` (`kill -9`) to guarantee the shutdown.
    To test whether your containerized application correctly handles `SIGTERM`, simply issue a `docker stop` against the container ID and check to see whether you get the `"task: non-zero exit (137)"`.
    This is not something to test in a production environment, as you can expect at least a brief interruption of service. Best practices would be to test in a development or test Docker environment.

2. The application hit an OOM (out of memory) condition.
    With regards to OOM condition handling, review the node's kernel logs to validate whether this occurred. This would require knowing which node the failed container was running on, or proceed with checking all nodes.
    Run something like this on your node(s) to help you identify whether you've had a container hit an OOM condition:

    ```
    journalctl -k | grep -i -e memory -e oom
    ```

    Another option would be to inspect the (failed) container:

    ```
    docker inspect <container ID>
    ```

    Review the application's memory requirements and ensure that the container it's running in has sufficient memory. Conversely, set a limit on the container's memory to ensure that wherever it runs, it does not consume memory to the detriment of the node.

    If the application is Java-based, you may want to review the maximum memory configuration settings.

## References

- [`docker run` command line options ](https://docs.docker.com/engine/reference/commandline/run/#options)
- [Specify hard limits on memory available to containers (-m, –memory)](https://docs.docker.com/engine/reference/commandline/run/#specify-hard-limits-on-memory-available-to-containers--m-memory)
