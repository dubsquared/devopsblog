---
layout: post
title: "Speed up MySQL with Redis"
date: 2013-04-29 8:00
comments: true
author: Hart Hoover
published: true
categories: 
- Redis
- Cloud Databases
---
Adding Redis to your application stack is a fantastic way to gain speed with existing applications. Many of our customers aren't running the latest and greatest new hotness NoSQL-using cloud thing. A lot of them port over a full stack of an existing applications that once only existed on bare metal servers, or use a hybrid environment with a big MySQL configuration on bare metal with web/app servers in the cloud.

If a customer wants to do the former, typically we advise that they use caching... EVERYWHERE. This is most important in front of the database. Setting up Redis in front of MySQL as a cache is a great way to improve the speed of your application.<!--More-->

##What's Redis?

From the "[Introduction to Redis](http://redis.io/topics/introduction)" page at [redis.io](http://redis.io):

> Redis is an open source, BSD licensed, advanced key-value store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets and sorted sets. You can run atomic operations on these types, like appending to a string; incrementing the value in a hash; pushing to a list; computing set intersection, union and difference; or getting the member with highest ranking in a sorted set. In order to achieve its outstanding performance, Redis works with an in-memory dataset. Depending on your use case, you can persist it either by dumping the dataset to disk every once in a while, or by appending each command to a log. Redis also supports trivial-to-setup master-slave replication, with very fast non-blocking first synchronization, auto-reconnection on net split and so forth. Other features include Transactions, Pub/Sub, Lua scripting, Keys with a limited time-to-live, and configuration settings to make Redis behave like a cache. You can use Redis from most programming languages out there. Redis is written in ANSI C and works in most POSIX systems like Linux, *BSD, OS X without external dependencies. Linux and OSX are the two operating systems where Redis is developed and more tested, and we recommend using Linux for deploying. Redis may work in Solaris-derived systems like SmartOS, but the support is best effort. There is no official support for Windows builds, but Microsoft develops and maintains a Win32-64 experimental version of Redis.

Want to try Redis? Check out [http://try.redis.io/](http://try.redis.io/)

##Why Redis?

There is a [ton](http://www.quora.com/Redis-vs-Memcached-which-one-should-I-use-for-a-web-based-application) of [debate](http://stackoverflow.com/questions/2873249/is-memcached-a-dinosaur-in-comparison-to-redis) out there on whether to use Redis or Memcached. Both are great, but Redis provides a few features over Memcached:

* Persistence: if you restart the memcached service, you have to warm up your cache again.
* Key/Value vs. Objects: Redis is more advanced in what it can store
* Selective deletion of cached items

##How do I set this up?

You can [install Redis](http://redis.io/download) on a Cloud Server if you like, but I recommend setting up a Redis instance at [RedisToGo](http://redistogo.com/). The instances are configured with Redis already, and RedisToGo makes Redis easy to scale.

Once Redis is installed and online, you need to configure your application to use it. 