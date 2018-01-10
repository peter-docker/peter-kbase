---
type: kbase
version: 5
title: "How do I use API V2 with DTR?"
summary: "To use the V2 Registry API with Docker Trusted Registry, use the following steps: Set the following environment variables: curl -u $username:$password '$host/auth/v2/token/?service=$host&scope=repository:$reponame:pull' Notice the `token` being returned in JSON. Paste that in JWT and verify that the `pull` permission exists. Then set the token: Finally test if the manifest is returned: curl -H 'Authorization: Bearer $token' 'https://$host/v2/$reponame/manifests/$tag' The manifests are shown:"
dateCreated: "Thu, 07 Apr 2016 21:43:56 GMT"
dateModified: "Fri, 29 Sep 2017 18:29:38 GMT"
dateModified_unix: 1506709778
upvote: 6
internal: no
author: "sabin.basyal"
platform:
  - linux
testedon:
tags:
  - API
product:
  - EE
---

To use the V2 Registry API with Docker Trusted Registry, use the following steps:

## Step 1

Set the following environment variables:

    username=admin
    password=<^><my-password><^^>
    host=<^>dtr.example.com<^^>
    
    reponame=admin/java-hello-world
    
    tag=latest

### Step 2

Get the tokens:

    curl -u $username:$password "$host/auth/v2/token/?service=$host&scope=repository:$reponame:pull"

Notice the `token` being returned in JSON. Paste that in [JWT](./images/Screenshot_2016-04-06_16.10.23.png)

Then set the token:

    token=eyJhbGciOiJFUzI1NiIsImp3ayI6eyJjcnYiOiJQLTI1NiIsImtpZCI6Ik5OWlg6S0g3VjpGQkpUOjRCQUg6V0VZTDpGMlY0OlJIV0o6QURGMzpVQUFFOjdKRVA6NlRBVTpGQ0FPIiwia3R5IjoiRUMiLCJ4IjoiekFYQURJOWcxMkx1NW1jMVBvZVlnNW9zX2UyeDZDcTk5UVpSQzFBTmZOcyIsInkiOiJ5U2JHd09ZLWpsbFkydWo2dUlrTXc5cm5SZi1YU0RNS2lfNlkzNmhLcjYwIn0sInR5cCI6IkpXVCJ9.eyJhY2Nlc3MiOlt7InR5cGUiOiJyZXBvc2l0b3J5IiwibmFtZSI6ImFkbWluL2phdmEtaGVsbG8td29ybGQiLCJhY3Rpb25zIjpbInB1bGwiXX1dLCJhdWQiOiJkdHIuc2FiaW5iYXN5YWwuY29tIiwiZXhwIjoxNDU5OTgzMjUyUCJpYXQiOjE0NTk5ODI5NTIsImlzcyI6ImR0ci5zYWJpbmJhc3lhbC5jb20iLCJqdGkiOiJJTWt0WWNSY1dOVXJZUDRSNTBMZSIsIm5iZiI6MTQ1OTk4Mjk1Miwic3ViIjoiYWRtaW4ifQ.K76uZMHVHiq7CtYPZSpJwfCcV5QGH2ZmMTV8S3TLTGcJdfdxc8rWyE8F1xhjngIrSk2YYntgu3jwg2BLZNz0wg

## Step 3

Finally test if the manifest is returned:

    curl -H "Authorization: Bearer $token" "[https://$host/v2/$reponame/manifests/$tag](https://$host/v2/$reponame/manifests/$tag "https://$host/v2/$reponame/manifests/$tag")"

The manifests are shown:

    {
    
       "name": "admin/java-hello-world",
    
       "tag": "latest",
    
       "architecture": "amd64",
    
       "fsLayers": [
    
          {
    
             "blobSum": "sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4"
    
          },
    
          {
    
    ...
    
    ...
    
       "schemaVersion": 1,
    
       "signatures": [
    
          {
    
             "header": {
    
                "jwk": {
    
                   "crv": "P-256",
    
                   "kid": "LDGX:VCCH:O36I:BTIZ:VU4Z:7UIW:OCOG:UDFA:LMVH:GJBQ:NTLY:ZITJ",
    
                   "kty": "EC",
    
                   "x": "mii7zsB5a-wzuxh7q1D-ngdE-qsr_zuG7fx-r_XZ4fw",
    
                   "y": "tkXiuXUa5HoavLxNwUQqKM5IeW-Q4A62Kun68nkYcFY"
    
                },
    
                "alg": "ES256"
    
             },
    
             "signature": "H939ST-lsPnc1AIfvkK-S5wN4mUe6VNXgRn3e1Py96awiWWHHwakz2g3_iXrI78PWH3F2i-YbHpWwmpReSXg1w",
    
             "protected": "eyJmb3JtYXRMZW5ndGgiOjM0MTQ2LCJmb3JtYXRUYWlsIjoiQ24wIiwidGltZSI6IjIwMTYtMDItMDJUMDA6MzM6MjRaIn0"
    
          }
    
       ]
    
    }
