---
type: kbase
version: 4
title: "Unable to connect to Docker Engine locally via namedpipe"
summary: "Connecting to the local engine fails via named pipe, but it works from remote machine or locally via network connection."
dateCreated: "Fri, 14 Apr 2017 23:03:25 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "eiichik"
platform:
  - windows
testedon:
tags:
  - networking
product:
---

## Issue

You can connect to Docker Engine via HTTP or named pipe, but you are unable to connect to Docker Engine from the local machine.

## Possible Cause

Not being able to connect to Docker Engine from the local machine might be caused by the named pipe being blocked.

## How to Verify

This command works on the same machine because it uses HTTP to connect instead of named pipe:  

`docker -H localhost info`

However, if you look at the output from `docker info` (which uses named pipe), if you see a golan error similar to the following containing `configureNpipeTransport`, there is a connection error to the named pipe:

    Exception 0xc0000005 0x1 0x4bdb404 0x180008910 
    PC=0x180008910 
    
    syscall.Syscall(0x7ff8dcdfdc40, 0x2, 0x40c, 0x3, 0x0, 0x0, 0x0, 0x410) 
    /usr/local/go/src/runtime/syscall_windows.go:163 +0x6b 
    github.com/docker/docker/vendor/github.com/Microsoft/go-winio.setFileCompletionNotificationModes(0x40c, 0x403, 0x0, 0xffffffff) 
    /go/src/github.com/docker/docker/vendor/github.com/Microsoft/go-winio/zsyscall_windows.go:88 +0x6f 
    github.com/docker/docker/vendor/github.com/Microsoft/go-winio.makeWin32File(0x40c, 0xc04240b890, 0x0, 0x0) 
    /go/src/github.com/docker/docker/vendor/github.com/Microsoft/go-winio/file.go:78 +0xd8 
    github.com/docker/docker/vendor/github.com/Microsoft/go-winio.DialPipe(0xc042117108, 0x16, 0xc04240b980, 0x0, 0x1120920, 0xc041fdf5ff, 0xc04240b9f8) 
    /go/src/github.com/docker/docker/vendor/github.com/Microsoft/go-winio/pipe.go:188 +0x537 
    github.com/docker/docker/vendor/github.com/docker/go-connections/sockets.DialPipe(0xc042117108, 0x16, 0x773594000, 0xc042082001, 0x1000000000010, 0x100, 0x100) 
    /go/src/github.com/docker/docker/vendor/github.com/docker/go-connections/sockets/sockets_windows.go:26 +0x46 
    github.com/docker/docker/vendor/github.com/docker/go-connections/sockets.<^>configureNpipeTransport<^^>.func1(0xba718f, 0x3, 0xc0423eb4e0, 0x19, 0x60, 0x0, 0xc042412001, 0xc04240e180)

The error message shows that connection to named pipe is failing. The extra option `-H localhost` tells the Docker client to connect via the network, so it works as expected. This can happen either because the named pipe was not created properly or because something is blocking the connection to named pipe.

Use the following Powershell command verify that named pipe for Docker Daemon is present:

    PS C:\Users\Administrator> [System.IO.Directory]::GetFiles("\\.\\pipe\\") | Select-String "docker"

If named pipe is present, the response should look like this:

    \\.\\pipe\\docker_engine

## How to Fix

Any software that blocks the named pipe can cause this error. For example, make sure you do not have "Web Companion from Lavasoft" installed on the server. It is known to block the named pipe connection.
