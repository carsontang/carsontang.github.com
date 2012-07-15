---
layout: post
title: "MongoDB Cheatsheet"
category: database
tags: [mongo, database]
---
{% include JB/setup %}

####Show a list of all databases
    > show dbs

####Use the database named "cheatsheet"
    > use cheatsheet

####Delete the database named "cheatsheet"
    > use cheatsheet
    > db.dropDatabase()

####Show a database's collections
    > show collections

####Show number of items in the collection "testcoll"
    > db.testcoll.count()
    
###Info
<ul>
<li>If no database is selected on startup, the default database is "test".</li>
<li>Databases and collections are created only when documents are first inserted.</li>
</ul>