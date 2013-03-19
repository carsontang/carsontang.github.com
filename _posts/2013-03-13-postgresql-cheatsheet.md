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
{% highlight psql %}
pg=# CREATE DATABASE sample_app_test
{% endhighlight %}
### Drop a database
    $ dropdb "sample_app_test"

### Drop a database via psql
{% highlight psql %}
pg=# DROP DATABASE sample_app_test
{% endhighlight %}
### Run command on psql without logging in
    $ psql -c "\l"

### Connect to a database
{% highlight psql %}
pg=# \c sample_app_test
{% endhighlight %}   
### Start psql with a database
    $ psql sample_app_development

### List all databases within psql
    postgres=# \l

### List all tables within a database
    sample_app_development=# \d

### Create a table
{% highlight psql %}
pg=# CREATE TABLE cities (
pg(#   city_code char(2) PRIMARY KEY,
pg(#   city_name text UNIQUE
pg(# );
{% endhighlight %}
### Insert a row
{% highlight psql %}
pg=# INSERT INTO cities (city_code, city_name)
pg-# VALUES ('sv', 'Sunnyvale'), ('mv', 'Mountain View');
{% endhighlight %}
### Insert a row (shorthand)
{% highlight psql %}
pg=# INSERT INTO cities
pg-# VALUES ('sv', 'Sunnyvale'), ('mv', 'Mountain View');
{% endhighlight %}
### Update a row
{% highlight psql %}
pg=# UPDATE users
pg-# SET is_approved = true
pg-# WHERE email = 'me@email.com';
{% endhighlight %}
### Drop a row
{% highlight psql %}
pg=# DELETE FROM users
pg-# WHERE email = 'bogus@bogus.net';
{% endhighlight %}
### Inner join
{% highlight psql %}
pg=# SELECT users.first_name, users.last_name
pg-# FROM users INNER JOIN offers
pg-# ON users.id = offers.user_id;
{% endhighlight %}
### Inner join with alias
{% highlight psql %}
pg=# SELECT u.first_name, u.last_name
pg-# FROM users u INNER JOIN offers o
pg-# ON u.id = o.user_id;
{% endhighlight %}
### Aggregate functions
{% highlight psql %}
pg=# INSERT INTO events (title, starts, ends, location_id)
pg-#    VALUES ('My 23rd Birthday Party',
pg-#    '2012-12-25 17:00:00', '2012-12-26 00:00:00', (
pg-#        SELECT location_id
pg-#        FROM locations
pg-#        WHERE name = 'Palo Alto'
pg-#    )
pg-# );
{% endhighlight %}