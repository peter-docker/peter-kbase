---
type: kbase
version: 2
title: "How can I configure the Docker container deployment with devices (--device option) through Docker Cloud?"
summary: "To use the --device option through the Docker Cloud web console, use Stack deployment (a Stackfile). An example of how devices are declared in a Stackfile is shown below: devices: - '/dev/ttyUSB0:/dev/ttyUSB0' - '/dev/ttyUSB1:/dev/ttyUSB1' For more information on Stack YAML reference, please refer to the documentation: https://docs.docker.com/docker-cloud/feature-reference/stack-yaml-reference/#devices"
dateCreated: "Mon, 18 Apr 2016 12:03:52 GMT"
dateModified: "Tue, 15 Nov 2016 18:07:01 GMT"
dateModified_unix: 1479233221
upvote: 0
internal: no
author: "fauzan.ariffin"
platform:
testedon:
tags:
product:
  - Hub
---

To use the 
--deviceoption through the Docker Cloud web console, use Stack deployment (a Stackfile). An example of how devices are declared in a Stackfile is shown below:

    devices:
     - "/dev/ttyUSB0:/dev/ttyUSB0"
     - "/dev/ttyUSB1:/dev/ttyUSB1"

For more information on Stack YAML reference, please refer to the documentation:  

<https://docs.docker.com/docker-cloud/feature-reference/stack-yaml-reference/#devices>