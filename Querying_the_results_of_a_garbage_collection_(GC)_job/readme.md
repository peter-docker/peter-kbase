---
type: kbase
version: 3
title: "Querying the results of a garbage collection (GC) job"
summary: "# get job job id from the last ${JOB_TYPE} job and send that to get the job logs curl -ks -X GET --header 'Accept: application/json' -u ${USERNAME}:${PASSWORD} 'https://${DTR_URL}/api/v0/jobs/$(curl -ks -X GET --header 'Accept: application/json' -u ${USERNAME}:${PASSWORD} 'https://${DTR_URL}/api/v0/jobs?action=${JOB_TYPE}&worker=any&running=any&start=0' | jq -r .jobs[0].id)/logs' | jq -r .[].Data"
dateCreated: "Wed, 30 Aug 2017 17:20:16 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "lenesto.page"
platform:
testedon:
tags:
product:
---

There are times where Docker support need to review the outcome of a previous Garbage Collection job. It can be used to verify orphaned data (to some extent), stale tags, manifests, etc. This article will show you how to query the results of the last completed GC job.

## Prerequisites

Before performing these steps, you must meet the following requirements:

* Review the garbage collection process: [https://docs.docker.com/registry/garbage-collection/](https://docs.docker.com/registry/garbage-collection/ "https://docs.docker.com/registry/garbage-collection/")

## Steps

To effectively query garbage collection data please see the commands below. Be sure to replace the following with the values for your configuration:

* <^>DTR_URL<^^>="dtr.example.com" (Use your DTR URL)

* <^>USERNAME<^^>="username"

* <^>PASSWORD<^^>="password"

* JOB_TYPE="job_type" (See link below for list of Job Types)

List of job types: [https://docs.docker.com/datacenter/d...obs/#job-types](https://docs.docker.com/datacenter/dtr/2.2/guides/admin/monitor-and-troubleshoot/troubleshoot-batch-jobs/#job-types "https://docs.docker.com/datacenter/dtr/2.2/guides/admin/monitor-and-troubleshoot/troubleshoot-batch-jobs/#job-types")

    ### job logs
    # get last job details
    curl -ks -X GET --header "Accept: application/json" -u ${<^>USERNAME<^^>}:${<^>PASSWORD<^^>} "https://${<^>DTR_URL<^^>}/api/v0/jobs?action=${<^>JOB_TYPE<^^>}&worker=any&running=any&start=0" | jq -r .jobs[0]
    
    # get job job id from the last ${JOB_TYPE} job and send that to get the job logs curl -ks -X GET --header "Accept: application/json" -u ${USERNAME}:${PASSWORD} "https://${DTR_URL}/api/v0/jobs/$(curl -ks -X GET --header "Accept: application/json" -u ${<^>USERNAME<^^>}:${<^>PASSWORD<^^>} "https://${<^>DTR_URL<^^>}/api/v0/jobs?action=${<^>JOB_TYPE<^^>}&worker=any&running=any&start=0" | jq -r .jobs[0].id)/logs" | jq -r .[].Data
