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

### Create a table
    pg=# CREATE TABLE cities (
    pg(#   city_code char(2) PRIMARY KEY,
    pg(#   city_name text UNIQUE
    pg(# );

### Insert a row
    pg=# INSERT INTO cities (city_code, city_name)
    pg-# VALUES ('sv', 'Sunnyvale'), ('mv', 'Mountain View');

### Insert a row (shorthand)
    pg=# INSERT INTO cities
    pg-# VALUES ('sv', 'Sunnyvale'), ('mv', 'Mountain View');

### Update a row
    pg=# UPDATE users SET is_approved = true WHERE email = 'me@email.com';

### Drop a row
    pg=# DELETE FROM users WHERE email = 'bogus@bogus.net';

### Inner join
    pg=# SELECT users.first_name, users.last_name FROM users INNER JOIN offers ON users.id = offers.user_id;
