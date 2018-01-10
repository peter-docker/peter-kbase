---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How do I use the swarm-rafttool for dumping Swarm's raft log and snapshots?
internal: yes        # if not yes, then optional
type: kbase
author: curtisz
product: EE
platform: linux #OPTIONAL, list all that apply
testedon: ucp-2.2.0
tags:
 - daemon
---
## Issue

When debugging Docker Swarm, it is sometimes helpful to check the actual raft logs for more information. These instructions will guide you through extracting these logs and the snapshots.

## Prerequisites

Before performing these steps, you must meet the following requirements:

- Ask in `#support` Slack channel for access to the `swarm-rafttool` tool archive.
- Ensure the Docker daemon is stopped on the host you want to check the raft logs for.

## Process

To use `swarm-rafttool`, once the Docker daemon is stopped:

Run `swarm-rafttool -d /var/lib/docker/swarm --redact dump-wal > somefile.txt` on the host.

This runs the program, tells it to look in the `/var/lib/docker/swarm` directory for the raft data, tells it to redact sensitive cluster information, then uses shell redirect to send the output to `somefile.txt`. The output of the command is a human-readable text file.

In addition, you should use the `dump-snapshot` subcommand with the same options to dump the raft snapshots as well. There is also a `dump-object` command, but this does not support the `--redact` flag in this build and will likely not provide much useful data by itself.

The raft log and snapshots are the total cluster state, and even with secrets and config data stripped out with the `--redact` flag, this data may still be sensitive. I recommend encrypting the data in an archive before sending it, and sharing the key over a secure channel.

## What's Next

Enjoy your raft logs and snapshots!
