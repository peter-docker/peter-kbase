---
type: kbase
version: 3
title: "ASCII codec can't encode character"
internal: no
author: "kizbitz"
dateModified: "2018-01-09T22:41:48-06:00"
dateModified_unix: 1515559308
platform:
testedon:
tags:
product:
  - Hub
uniqueid: KB000129
---
We should see a uniqueid created

TESTING TESTING AND MORE TESTING!!!

When deploying with a stack file with Docker Cloud, the following error message might occur:

```
Unexpected error executing docker command create_container: 'ascii' codec can't encode character 'some-character' in position 20: ordinal not in range(128)
```

If you get this error message, double check your stack file and make sure that there aren't any extra spaces or hidden characters.
