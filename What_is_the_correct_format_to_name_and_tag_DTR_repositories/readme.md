---
type: kbase
version: 5
title: "What is the correct format to name and tag DTR repositories?"
dateCreated: "Wed, 02 Aug 2017 05:30:05 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 0
internal: no
author: "jacquesg"
platform:
testedon:
  - dtr-2.3.3
tags:
product:
  - EE
---

DTR requires you to create the repo first before you can push to it (which is different than Docker Hub), so make certain your repo exists before pushing to it.

DTR only supports repository names and tags in the form of:

* `<^><hostname><^^>/<^><namepsace><^^>/<^><repository><^^>`
* `<^><hostname><^^>/<^><namepsace><^^>/<^><repository><^^>:<^><tag><^^>`

## Examples of pull tag and push

```
docker pull <^>busybox<^^>
docker login <^>10.0.2.15<^^> (sign in as admin)

docker tag <^>busybox 10.0.2.15/admin/hello-world<^^>
docker push <^>10.0.2.15/admin/hello-world<^^>

                    OR
                  
docker tag <^>busybox 10.0.2.15/admin/hello-world:1<^^>
docker push <^>10.0.2.15/admin/hello-world:1<^^>
```
