---
type: customerservice
version: 8
title: "How do I change my Docker username?"
summary: "To change your Docker username, make a copy of your existing images, request your old Docker account to be disabled, create a new Docker username, and restore your images and Automatic Builds to your new Docker account. Docker Support will then proceed to deactivate your account, remove the associated email addresses, and notify you when you can create a new account."
dateCreated: "Tue, 08 Dec 2015 00:46:37 GMT"
dateModified: "Thu, 30 Mar 2017 18:55:24 GMT"
dateModified_unix: 1490900124
upvote: 9
internal: no
author: "allenlai"
platform:
testedon:
tags:
product:
  - Hub
---

Account renames have been temporarily suspended due to system migration. To change your Docker username, make a copy of your existing images, request your old Docker account to be disabled, create a new Docker username, and restore your images and Automatic Builds to your new Docker account.

### Preparation

Before requesting your old Docker username to be disabled:

1. Download any images (and tags) you wish to keep by running the following command: `docker pull -a <image>`
2. [Delete all repositories](https://success.docker.com/Cloud/Solve/How_do_I_delete_a_repository%3F "How Do I Delete a Repository") associated with your account.
3. If you have an active Docker subscription, downgrade to the free plan.
4. If you have a license key, [download the key](/article/How_do_I_download_my_Docker_license_key%3F "How to download your Docker license key") from your Docker Hub account. The download link will no longer be available after the rename.
5. If you belong to an organization, remove yourself from the organization.
6. If you are the sole owner of an organization, either remove yourself from the organization or add someone to the "owners" team and then remove yourself from the organization.
7. Unlink your Github and Bitbucket accounts.

### Renaming

Once you have completed the preparation steps, contact Support to disable your Docker username. If you have a current subscription, go to the [Docker Support Site](https://support.docker.com "https://support.docker.com"). Otherwise, go to the [Customer Service Request](/Help "Customer Service") form.

Docker Support will then proceed to deactivate your account, remove the associated email addresses, and notify you when you can create a new account. Once your email address has been removed from the old account, you may then proceed with creating a new account using that email address.

### Repopulating

After creating your new account, you can restore images and add additional email addresses to the account.

1. Run `docker tag` for all of your images with your new account name.
2. Run `docker push` for each of your images to push to your new Docker Hub account.
3. If you created any Automated Builds, recreate them on the new account.
4. Add any additional email addresses to the account.