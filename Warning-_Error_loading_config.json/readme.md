---
type: kbase
version: 3
title: "Warning: Error loading config.json"
summary: "EOF error loading  config.json"
dateCreated: "Fri, 23 Jun 2017 17:45:56 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "jasonbivins"
platform:
  - windows
testedon:
tags:
  - error
product:
  - D4W
---

## Issue

When trying to run a Docker task or command on Windows 10, you receive the following error — in some cases the command completes:

    WARNING: Error loading config file:.docker\config.json - EOF

## Prerequisites

Before performing these steps, you must meet the following requirements:

* Windows 10
* Docker for Windows

## Steps

1. Locate the `config.json` file located under the `.docker` folder in your `Users` folder.
2. If the file is empty, add `{ }` to the file and save it.
3. Try the Docker task or command again.
