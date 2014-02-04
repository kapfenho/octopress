---
layout: post
title: "Responsive search forms with Redis database"
date: 2012-12-22 12:21
comments: true
categories: 
---
We all know them well: relational database system (RDBMS) -- like Oracle,
MySQL, PostgreSQL, etc.  Basically you can model and implement everything with them, they are stable and the standardized query language [SQL](http://en.wikipedia.org/wiki/Sql) is powerful.

But RDBMS may not be the answer to every question and they are not
designed to handle workload and data sizes brought by our beloved
Internet.

An somehow infelicitous buzz word groups alternatives of relational
databases: [NoSQL](http://nosql-database.org).  Seldom systems of such diversity were put together
in one category...

Recently I had the joy to use one of them again, one that I really like:
Redis.  Firm implementation, beautiful interface, and easy to use.

Check out the web site [web site](http://redis.io), including an
[interactive tutorial](http://try.redis-db.com).

We've created a prototype for a customer, helping them to write a common
customer search form over multiple CRM systems.  Since searches on
multiple legacy systems via middleware layers may not be very responsive, a caching database
sounded like a way to go.

This prototype you can find on
[Github](https://github.com/kapfenho/redis-cache). Updates
will follow, we are still working on it.

If you want to learn more about Redis, there is a nice and free 30 pages
book in PDF from Karl Seguin: [The Little Redis Book](http://openmymind.net/2012/1/23/The-Little-Redis-Book/). 

