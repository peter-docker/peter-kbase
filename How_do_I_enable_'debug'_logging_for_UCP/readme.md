---
type: kbase
version: 6
title: "How do I enable 'debug' logging for UCP?"
summary: "To enable debug logging for your UCP controllers, navigate in the Web UI to Admin Settings > Logs > LOG SEVERITY LEVEL, and select DEBUG. To confirm the setting has taken effect, check the arguments on the ucp-controller container for --debug To disable debug logging, perform these steps again, setting LOG SEVERITY LEVEL to INFO. To configure debug logging for the Docker daemon, please see How do I enable 'debug' logging of the Docker daemon?"
dateCreated: "Wed, 21 Jun 2017 19:28:52 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "trapier"
platform:
testedon:
  - ucp-2.1.4
tags:
product:
  - EE
---

To enable debug logging for your UCP controllers, navigate in the Web UI to **Admin Settings** > **Logs** > **LOG SEVERITY LEVEL**, and select **DEBUG**. The setting will take effect within about a minute.

> **Warning:** Applying a change to this setting will result in UCP controllers restarting and may briefly interrupt UCP availability.

![](./images/configure-logs-1.png)

To confirm the setting has taken effect, check the arguments on the `ucp-controller` container for `--debug`

    $ docker inspect ucp-controller --format '{{range .Args}}{{printf "%s\n" .}}{{end}}' | grep debug
    
    --debug

To disable debug logging, perform these steps again, setting **LOG SEVERITY LEVEL** to **INFO**.

This setting controls the log level for the following main UCP components:

* ucp-agent service
* ucp-auth-store containers
* ucp-controller containers
* ucp-hrm service
* ucp-kv containers

To configure debug logging for the Docker daemon, please see [How do I enable "debug" logging of the Docker daemon?](/article/How_do_I_enable_%22debug%22_logging_of_the_Docker_daemon%3F "How do I enable "debug" logging of the Docker daemon?")