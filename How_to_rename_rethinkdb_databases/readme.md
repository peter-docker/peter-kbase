---
# Refer to https://github.com/docker/kbase/blob/master/standards/readme.md for detailed description of metadata
title: How to rename rethinkdb databases
internal: no             
comment: ""
type: kbase               
author:  lenesto.page
product:       
  - ee         # EE (Docker EE - Basic, Standard, and Advanced)
testedon:           
  - ee-17.06.1-ee-2
  - ucp-2.2.3
platform:           
  - linux
tags:               
  - error
---
## Issue

UCP can encounter a situation in which the enzi database can exist twice in rethinkdb, thus not allowing UCP to successfully load. The error received throughout the UCP controller logs and UCP Auth logs is:

```
{"level":"fatal","msg":"Malformed configuration detected, unable to continue: unable to set top-level auth config: unable to set auth config property: unable to set property in database: gorethink: Database `enzi` is ambiguous; there are multiple databases with that name.
```

## Resolution

To rename a database in rethinkdb (specifically enzi in this case) please do the following:

1. Pull the jlhawn tool from the Docker Hub onto any manager:
   
   ```
    $ docker pull jlhawn/rethinkdb-repl
    
    ```
2. Access rethinkdb using the jlhawn tool:

    ```
    $ docker run --rm --net host -v ucp-auth-store-certs:/tls -it jlhawn/rethinkdb-repl localhost 

    ```
3. List the current databases in rethinkdb:

  ```
  $ r.db('rethinkdb').table('db_config') 

  ```

4. Rename the database:

  ```
  $ r.db('rethinkdb').table('db_config').get('id of second db').update({name: "name of second db"})

  ```

If UCP still doesn't load, this means you have renamed the wrong database. In this case, the enzi database was listed twice in rethinkdb with the same name, but different ids in rethink. If you rename the "official" database, UCP will not come up and will continue to error out. At this point go back and do the rename process again, this time renaming the other database to something else and the official database back to the original name.
