---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How can I find the last time a garbage collection (GC) job ran?
internal: no             # set to yes to keep it internal-only
comment: ""
type: kbase               # set to customerservice if applicable
author:  bryceryan-docker
product:       # Optional. Keep all that apply.
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           # Specific product and component version(s) that this issue was verified on / is guaranteed to work with. It may be applicable to a broader set of versions, ie UCP 2.x (which can be stated in the text), but this should be a list of the specific version(s) we tested with. Include the full version string used in the relnotes. Product abbreviations are the same as the "product" variable above. Valid component abbreviations are "ucp", "dtr", and "daemon" (engine).
  - ee-17.06.2-ee-3
  - ucp-2.2.0
  - dtr-2.3.0
platform:           # Optional. Keep all that apply.
  - linux
tags:               # Optional. List of tags associated with this issue. MUST BE one or more of the following. Do not use other tags.
  - logging
  - monitoring
  - storage
---
## Issue

Cluster operators and Docker Technical Support may need to verify the last time a DTR Garbage Collection (GC) job ran. There
are many circumstances where a cluster in trouble or a customer may simply want to know that a GC operation occurred in the
recent past. This article shows a fairly quick query to determine when the last GC job ran. More complex queries can be used
to determine more detailed status.

## Resolution

To effectively query garbage collection data, use the following commands:

1. At the command line, set these environment variables in your shell, being sure to replace with your values:
    ```
    DTR_URL="<^>dtr.example.com<^^>" (Use the DTR URL that can be seen from the node where you run this command)
    USERNAME="<^>username<^^>"
    PASSWORD="<^>password<^^>"
    ```

2. After creating or updating those environment variables, run the following command:
    ```
    curl -ks -X GET --header "Accept: application/json" -u ${USERNAME}:${PASSWORD} "https://${DTURL}/api/v0/jobs?action=gc&worker=any&running=any&start=0&limit=0"
    ```

Here is some sample output:
  ```
  $ curl -ks -X GET --header "Accept: application/json" -u ${USERNAME}:${PASSWORD} "https://${DTURL}/api/v0/jobs?action=gc&worker=any&running=any&start=0&limit=0" | jq .jobs[]
{
  "id": "1103541f-0575-4221-a88f-6daf14480700",
  "retryFromID": "1103541f-0575-4221-a88f-6daf14480700",
  "workerID": "c94f62549b17",
  "status": "done",
  "scheduledAt": "2017-05-18T23:27:12.719Z",
  "lastUpdated": "2017-05-18T23:27:35.671Z",
  "action": "gc",
  "retriesLeft": 2,
  "retriesTotal": 2,
  "capacityMap": {},
  "parameters": {},
  "deadline": "",
  "stopTimeout": "30s"
}
```

## What's Next

See the following kbase article for a more thorough treatment of GC jobs: 

- [Querying GC Job Results](https://success.docker.com/article/Querying_the_results_of_a_garbage_collection_(GC)_job)
