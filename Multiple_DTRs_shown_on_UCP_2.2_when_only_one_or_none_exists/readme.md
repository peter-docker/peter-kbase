---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: "Multiple DTRs shown on UCP 2.2 when only one or none exists"
internal: no            
comment: ""
type: kbase
author:  cherylq26
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
  - ee-17.06.2-ee-3
  - ucp-2.2.3
platform:           # Optional. Keep all that apply.
  - linux
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - error
  - uninstalling
---
## Issue

On your UCP UI, you may find stale DTR entries under <^>Username<^^> > **Admin Settings** > **Docker Trusted Registry** page. This article explains how you can remove those stale DTR entries from the UCP UI through the command line.

![](./images/stale-dtr-entries.png)

## Prerequisites

Before performing these steps, you must meet the following requirements:
- Universal Control Plane 2.2.x
- Docker EE 17.06 and higher

## Resolution

Perform the following steps using the UCP configuration file to remove the stale DTR entry/entries:

1. Run the following commands to view your current UCP configuration file:

    ```
    # CURRENT_CONFIG_NAME will be the name of the currently active UCP configuration 
    CURRENT_CONFIG_NAME=$(docker service inspect ucp-agent --format '{{range .Spec.TaskTemplate.ContainerSpec.Configs}}{{if eq "/etc/ucp/ucp.toml" .File.Name}}{{.ConfigName}}{{end}}{{end}}') 

    # Collect the current config with `docker config inspect` 
    docker config inspect --format '{{ printf "%s" .Spec.Data }}' $CURRENT_CONFIG_NAME > ucp-config.toml
    ```
2. Edit the `ucp-config.toml` file and remove the `[[registries]]` section(s) for the stale DTR entry/entries at the bottom of the file. If you wish to remove all DTR entries from the UCP UI, your `ucp-config.toml` file should end with something similiar to the following: 

    ```
    [trust_configuration] 
    require_content_trust = false
    ```
3. Run the following commands to create and apply the configuration from the file:

    ```
    # NEXT_CONFIG_NAME will be the name of the new UCP configuration 
    NEXT_CONFIG_NAME=${CURRENT_CONFIG_NAME%%-*}-$((${CURRENT_CONFIG_NAME##*-}+1)) 

    # Create the new swarm configuration from the file ucp-config.toml 
    docker config create $NEXT_CONFIG_NAME ucp-config.toml 

    # Use the `docker service update` command to remove the current configuration and apply the new configuration to the `ucp-agent` service. 
    docker service update --config-rm $CURRENT_CONFIG_NAME --config-add source=$NEXT_CONFIG_NAME,target=/etc/ucp/ucp.toml ucp-agent
    ```
    After a few seconds, you should notice that a new `ucp-agent` is spun up.
4. Confirm that the stale DTR entries are removed on the UCP UI (<^>Username<^^> > **Admin Settings** > **Docker Trusted Registry**) page.

## Related Article

If you are on UCP 2.0, you may refer to the article [Multiple DTRs shown on UCP 2.0 when only one or none exists](https://success.docker.com/article/Multiple_DTRs_shown_when_only_one_exists).
