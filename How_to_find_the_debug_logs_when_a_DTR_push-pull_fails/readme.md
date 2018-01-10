---
type: kbase
version: 4
title: "How to find the debug logs when a DTR push/pull fails"
summary: "If a push/pull to your DTR fails, look for information in the debug logs from the ucp-controller container. To enable debug logs on the UCP controller, go to the Settings page in the UCP UI. Under the Logs Settings tab, select DEBUG from the LEVEL dropdown and click the Enable Remote Logging button. While this runs, try the docker push/pull ... command that failed (using the Docker client configured to use the UCP cert bundle as before)."
dateCreated: "Sat, 09 Jul 2016 00:02:27 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "sabin.basyal"
platform:
testedon:
tags:
  - logging
product:
  - EE
---

If a push/pull to your DTR fails, look for information in the debug logs from the `ucp-controller` container.

To enable debug logs on the UCP controller, go to the **Settings** page in the UCP UI. Under the **Logs** Settings tab, select **DEBUG** from the **LEVEL** dropdown and click the **Enable Remote Logging** button. It is okay to leave the other fields blank.

Next, access a shell on the UCP Controller host. Run`docker``logs -f ucp-controller 2>&1 | ucp-controller.log`. While this runs, try the `docker push/pull ...` command that failed (using the Docker client configured to use the UCP cert bundle as before). It will fail again, but now there should be debug logs on the controller container as a result.

Next, press `Ctrl+C` in the other shell to interrupt the `docker logs -f ...` process. The `ucp-controller.log` file generated should include the debug logs from the failed push/pull.

> **NOTE:** If you have more than one controller, for example 3 controllers, then go directly to each controller and set it — in your browser, go to `[https://controller-ip-1](https://controller-ip-1 "https://controller-ip-1")`, `[https://controller-ip-2](https://controller-ip-2 "https://controller-ip-2")`, and `[https://controller-ip-3](https://controller-ip-3 "https://controller-ip-3")` and enable debugging on each one.
