---
type: kbase
version: 3
title: "When installing Docker UCP the 'Check your firewall settings' error occurs"
summary: "DEBU[0003] Failed again to launch port test container: Error response from daemon: driver failed programming external connectivity on endpoint ucp-port-check-12376 (62c342d0feae5876ad4660eddcdc818c9e8eb47473eeef524639f62c05b16965): (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 12376 -j DNAT --to-destination 172.17.0.8:2376 ! -i docker0: iptables: No chain/target/ match by that name. (exit status 1))"
dateCreated: "Fri, 05 May 2017 19:48:07 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "squizzi"
platform:
  - linux
testedon:
tags:
  - installing
  - error
  - networking
product:
  - EE
---

When installing UCP the following error appears when access to required ports is denied:

    FATA[0036] the following required ports are blocked on your host: 12387, 4789, 12381, 12386, 443, 2376, 12379, 12380, 12376, 12382, 12383, 12384, 12385.  Check your firewall settings

The error may be prefaced by the following messages:

    DEBU[0003] Failed again to launch port test container: Error response from daemon: driver failed programming        external connectivity on endpoint ucp-port-check-12376       (62c342d0feae5876ad4660eddcdc818c9e8eb47473eeef524639f62c05b16965):  (iptables failed: iptables --wait -t nat -A      DOCKER -p tcp -d 0/0 --dport 12376 -j DNAT --to-destination 172.17.0.8:2376 ! -i docker0: iptables: No chain/target/  match by that name. (exit status 1))

## Resolution

The error message produced in this issue is a general message that appears when any of the port checks UCP attempts to make during installation fail.

1. To troubleshoot this issue, enable debugging during the UCP install with the `-D` flag: 
    
        $ sudo docker run --rm -it \
        --name ucp -v /var/run/docker.sock:/var/run/docker.sock \
        docker/ucp install -i --host-address 10.0.168.195 -D
2. The debugging output should give you an idea of which ports are failing: 
    
        DEBU[0003] Failed again to launch port test container: Error response from daemon: driver failed programming external connectivity on endpoint ucp-port-check-12376 (62c342d0feae5876ad4660eddcdc818c9e8eb47473eeef524639f62c05b16965):  (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 12376 -j DNAT --to-destination 172.17.0.8:2376 ! -i docker0: iptables: No chain/target/match by that name.
         (exit status 1))
3. Check your firewall settings to make sure the required ports are opened.
4. You might also need to flush existing iptables then restart Docker Engine to refill the iptables with the correct `DOCKER` chain entries: 
    
        $ sudo iptables -F && sudo systemctl restart docker.service
5. Finally, attempt to install UCP again.

## What's Next

If these steps do not resolve your issue, see: [UCP is unable to install because of a port conflict](/article/UCP_is_unable_to_install_because_of_a_port_conflict)
