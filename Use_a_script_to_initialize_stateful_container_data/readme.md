---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to use an entrypoint script to initialize container data at runtime
type: kbase
author:  adamancini
product:
  - EE
platform:
  - linux
testedon:
  - ee-17.06.2-ee-3 
  - ucp-2.2.0
  - dtr-2.3.0
tags:
  - daemon
---

## Issue

How do I initialize/seed data into my container at runtime, before the daemon process starts up in my container?

## Resolution

There's a very common pattern used to initialize stateful data in a container at runtime.  Use a shell script as the ENTRYPOINT of a container, and execute the necessary setup steps before passing control to a long-running process.

1. Create a shell script with the following contents (named `docker-entrypoint.sh` in our example):
```
#!/bin/bash
set -e

if [ "$1" = 'postgres' ]; then
    chown -R postgres "$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"
fi

exec "$@"
```
2. Your Dockerfile should then call this script. For example, the following Dockerfile is an excerpt from the [postgres](https://github.com/docker-library/postgres/blob/f34b7cb79c4209a67b573f3bc4bc7827d69800e1/10/Dockerfile) image:
```
FROM debian:stretch
...
COPY docker-entrypoint.sh /usr/local/bin/
RUN ln -s usr/local/bin/docker-entrypoint.sh / # backwards compat
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["postgres"]
```

You can see that when the container starts up, the command portion is interprated to be `sh -c 'docker-entrypoint.sh postgres'`.

The script is invoked and given the argument `postgres`.  The script checks if the first argument sent to it is equal to the string `postgres`, and if so, executes a series of instructions to set up a database.  Finally, the `exec` shell construct is invoked, so that the final command given becomes the container's PID 1.  `$@` is a shell variable that means "all the arguments", so that leaves us with simply `exec postgres`.

## What's Next

See [Dockerfile Best Practices](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#entrypoint)
