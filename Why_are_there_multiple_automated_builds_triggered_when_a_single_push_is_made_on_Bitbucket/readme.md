---
type: kbase
version: 8
title: "Why are there multiple automated builds triggered when a single push is made on Bitbucket?"
summary: "There are multiple automated builds triggered when a single push is made on Bitbucket. This can be resolved by checking the number of services/webhooks linked in the Bitbucket account settings. You can see more than one automated build entry made for a single push made. Under ‘Workflow’, click on Services and ensure that there are no services configured as displayed in the screenshot below: Next, click on Webhooks and ensure that only one entry is configured as displayed in the screenshot below:"
dateCreated: "Mon, 18 Apr 2016 07:55:10 GMT"
dateModified: "Thu, 11 May 2017 09:50:48 GMT"
dateModified_unix: 1494496248
upvote: 0
internal: no
author: "fauzan.ariffin"
platform:
testedon:
tags:
product:
  - Hub
---

## Overview

There are multiple automated builds triggered when a single push is made on Bitbucket. This can be resolved by checking the number of services/webhooks linked in the Bitbucket account settings.

## Symptoms

You can see more than one automated build entry made for a single push made.

**![](./images/**![](https://lh4.googleusercontent.com/)

## Resolution

1. Log into your Bitbucket account.
2. Navigate to the repository settings page to check for additional webhooks/services configured.
3. Under ‘**Workflow**’, click on **Services** and ensure that there are no services configured as displayed in the screenshot below:  
    
    **![](./images/    **![](https://lh5.googleusercontent.com/)
4. Next, click on **Webhooks** and ensure that only one entry is configured as displayed in the screenshot below:  
    
    **![](./images/    **![](https://lh4.googleusercontent.com/)
5. If there are any additional entries, remove them by clicking the **Delete** button.