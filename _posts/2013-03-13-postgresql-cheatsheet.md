---
layout: post
title: "PostgreSQL Cheatsheet"
category: PostgreSQL
tags: [PostgreSQL]
---
{% include JB/setup %}

### Create a database
    $ createdb sample_app_test

### Create a database via psql
    pg=# CREATE DATABASE sample_app_test

### Drop a database
    $ dropdb "sample_app_test"

### Drop a database via psql
    pg=# DROP DATABASE sample_app_test

### Run command on psql without logging in
    $ psql -c "\l"

### Connect to a database
    pg=# \c sample_app_test
    
### Start psql with a database
    $ psql sample_app_development

### List all databases within psql
    postgres=# \l

### List all tables within a database
    sample_app_development=# \d


