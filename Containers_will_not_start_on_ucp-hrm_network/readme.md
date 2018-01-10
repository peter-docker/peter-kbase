---
type: kbase
version: 11
title: "Containers will not start on ucp-hrm network"
summary: "The resolution involves provisioning additional address space and can take one of two forms: deploying an additional network HRM network, or reconfiguring the existing ucp-hrm network with a larger subnet. Reconfiguring the existing network requires removing all services attached to the network, but allows services to continue referencing the same ucp-hrm network name. Note: the following procedure requires removing services attached to the ucp-hrm network in order to recreate the network."
dateCreated: "Wed, 23 Aug 2017 16:40:08 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "trapier"
platform:
testedon:
  - ucp-2.2.0
tags:
  - error
product:
  - EE
---

When deploying stacks or services to the ucp-hrm network, an issue may occur where no container is created under the service. `docker service ls` will show fewer tasks scheduled than expected, and tasks scheduled under the service will show `State: "new"` with `Message: "created"`.

The cause of this issue is IP address exhaustion on the ucp-hrm network. To confirm this root cause, watch the Docker daemon logs on the swarm leader for the following error while deploying a service:

    dockerd[1148]: time="2017-08-18T16:52:57.658494321Z" level=error msg="Failed allocation for service22vv3642wp8txhbu4t6uk2agx" error="could not find an available IP while allocating VIP" module=node node.id=pujf6ei

## Resolution

The default subnet size for ucp-hrm is `/24` (~250 IP addresses). The resolution involves provisioning additional address space and can take one of two forms: deploying an additional network HRM network, or reconfiguring the existing `ucp-hrm` network with a larger subnet. Both approaches involve temporary downtime for the HRM service. Reconfiguring the existing network requires removing all services attached to the network, but allows services to continue referencing the same `ucp-hrm` network name.

### Approach 1: Deploy a new HRM network

To deploy a new HRM network, perform the following steps through a [UCP client bundle](https://docs.docker.com/datacenter/ucp/2.2/guides/user/access-ucp/cli-based-access/ "https://docs.docker.com/datacenter/ucp/2.2/guides/user/access-ucp/cli-based-access/"):

1. Create a new `/16` overlay network with the `com.docker.ucp.mesh.http` label. The `subnet`, `gateway`, and name can be configured as desired.
    
        docker network create -d overlay  --subnet=<^>10.1.0.0/16<^^> --gateway <^>10.1.0.1<^^> --label com.docker.ucp.mesh.http=true <^>hrmnet<^^>

2. [Toggle the HRM service off and then on on the UCP UI](https://docs.docker.com/datacenter/ucp/2.2/guides/admin/configure/use-domain-names-to-access-services/#enable-the-http-routing-mesh "https://docs.docker.com/datacenter/ucp/2.2/guides/admin/configure/use-domain-names-to-access-services/#enable-the-http-routing-mesh").

3. Attach new services to the new network name (`hrmnet` in this example).

### Approach 2: Reconfigure the existing ucp-hrm network

> Note: the following procedure requires removing services attached to the ucp-hrm network in order to recreate the network. This will involve application downtime and requires the ability to re-deploy stacks and services running on that network.

Perform the following steps through a [UCP client bundle](https://docs.docker.com/datacenter/ucp/2.2/guides/user/access-ucp/cli-based-access/ "https://docs.docker.com/datacenter/ucp/2.2/guides/user/access-ucp/cli-based-access/").

1. Remove all services on the `ucp-hrm` network:
    
        HRM_NETWORK_ID=$(docker network ls --no-trunc -qf name=ucp-hrm);
        docker service ls -q | \
              xargs docker service inspect --format '{{ range .Spec.TaskTemplate.Networks }}{{ if eq '\"$HRM_NETWORK_ID\"' .Target }}{{ $.Spec.Name }} {{ end }}{{ end }}'  |\
          xargs docker service rm;

2. Remove the `ucp-hrm` network:
    
        docker network rm ucp-hrm;

3. Re-create the `ucp-hrm` network with a `/16` network size. The `subnet` and `gateway` can be configured as desired.
    
        docker network create -d overlay --subnet=
        10.1.0.0/16 --gateway 
        10.1.0.1 --label com.docker.ucp.mesh.http=true ucp-hrm

4. [Re-enable ucp-hrm in the UCP Admin UI](https://docs.docker.com/datacenter/ucp/2.2/guides/admin/configure/use-domain-names-to-access-services/#enable-the-http-routing-mesh "https://docs.docker.com/datacenter/ucp/2.2/guides/admin/configure/use-domain-names-to-access-services/#enable-the-http-routing-mesh").

5. Re-deploy services removed in step 1.
