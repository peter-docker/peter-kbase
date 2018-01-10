---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: Why does my node reboot without notice?
internal: no             
comment: ""
type: kbase              
author:  romainbelorgey
product:       
  - ee         
testedon:           
platform:          
  - linux
tags:               
  - daemon
---

## Issue

A server, manager, or worker node reboots without notices and without clear explanation.

## Resolution

To debug this node reboot, use these methods to gather more information.

### Use the last Command

Use the command `last`. It shows who has recently used the server and logged in and out date/time.
    
For example, this will filter only the reboot:
    
```
last | grep -i reboot
```

The command `last -x` normally filter only the reboot times.

### Check the Log Files

Check the logs. Depends on your system, you might need to check the logs on journald or inside the `/var/log` directory or both.

For journald, use the command `journalctl`. The [documentation] (https://www.freedesktop.org/software/systemd/man/journalctl.html) explains all the options and how to filter the results.

Look inside your `/var/log` directory. You can grep for keywords such as reboot and panic:

```
grep -ri "reboot" /var/log/*
grep -ri "shut" /var/log/*
grep -ri "panic" /var/log/*
```

### Check for System Metrics

Also check for system metrics. It can help you find if it is related to system issues (load, filesystem full, out of memory, etc).
