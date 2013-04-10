---
layout: post
title: "Using Message Queues in Cloud Applications"
date: 2013-04-11 08:00
comments: true
author: Hart Hoover
published: true
categories: 
- Five Pillars
- Cloud Tools
---
{% img right a/pillars/pillar.png 160 160 %}
In Wayne Walls' [recent post on parallel computing](http://www.rackspace.com/blog/pillars-of-cloudiness-no-1-parallel-computing/), message queues are mentioned as a way to achieve parallel computing in an application. In this post, we will dive into the different message queues out there and how to implement a message queue in an application.

##Which queue do I use?

There are several queues to choose from, each with benefits and drawbacks. 

###RabbitMQ
The most popular is [RabbitMQ](http://www.rabbitmq.com/), due to multiple operating system support (even Windows!) and a plethora of [clients and tools](http://www.rabbitmq.com/devtools.html) that support RabbitMQ. RabbitMQ also implements a common standard: the Advanced Message Queuing Protocol (AMQP).

RabbitMQ also powers the messaging between services inside the Rackspace Open Cloud infrastrcuture today.

###ActiveMQ

###ZeroMQ

###Marconi

[Marconi](https://wiki.openstack.org/wiki/Marconi) is a new OpenStack project to create a multi-tenant cloud queuing service. The aim is to create an open alternative to Amazon's SQS (producer-consumer) and SNS (pub-sub) services, for use in applications that run on OpenStack clouds. Marconi is currently in development.

###IronMQ

[IronMQ](http://www.iron.io/mq) is an easy-to-use highly available message queuing service. IronMQ is for the person that doesn't want to manage their own queue servers. They provide an endpoint where you create queues and messages with a highly available backend. IronMQ uses HTTPS to transport messages.

##How to configure RabbitMQ to handle messages

