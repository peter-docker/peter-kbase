---
type: kbase
version: 3
title: "How to properly restart DTR 2.3.x"
summary: "After completing these steps, you will have the ability to properly restart DTR on 2.3.x versions of the product. Choose the application with the replica id that corresponds to the replica to be restarted. On the right choose the Inspect Resources pull down menu, and then Containers from the drop down. Check the boxes for all DTR containers. On the right side, choose the Actions pull-down menu, and then Restart from the drop down. From the CLI of the replica host run the following:"
dateCreated: "Tue, 26 Sep 2017 13:44:08 GMT"
dateModified: "Wed, 04 Oct 2017 03:50:15 GMT"
dateModified_unix: 1507089015
upvote: 0
internal: no
author: "scasey"
platform:
  - linux
testedon:
  - dtr-2.3.0
tags:
product:
  - EE
---

After completing these steps, you will have the ability to properly restart DTR on 2.3.x versions of the product.

# Option 1

From the UCP UI:

![Screen Shot 2017-09-26 at 9.43.17 AM.png](./images/Screen_Shot_2017-09-26_at_9.43.17_AM.png)

1. Click **Stacks** in the left-side menu.
2. Choose the *application* with the replica id that corresponds to the replica to be restarted.
3. On the right choose the **Inspect Resources** pull down menu, and then **Containers** from the drop down.
4. Check the boxes for all DTR containers.
5. On the right side, choose the **Actions** pull-down menu, and then **Restart **from the drop down*.*

*![Screen Shot 2017-09-26 at 9.42.59 AM.png](./images/Screen_Shot_2017-09-26_at_9.42.59_AM.png)

# Option 2

From the CLI of the replica host run the following:

    $ sudo docker ps -qf name=dtr |xargs docker restart