---
layout: post
title: "Modular Application Design"
date: 2013-04-04 08:00
comments: true
author: Hart Hoover
published: true
categories: 
- Cloud Servers
- Five Pillars
- Cloud Tools
---
{% img right a/pillars/pillar.png 160 160 %}
Wayne Walls posted a great article on the importance of modularity in cloud application design.<!--More--> Typically, when we speak about modular design we mean something like this:

(arch of wordpress goes here)

As you can see, we have a typical web application that is indeed very modular. It has a few Varnish caching servers, a few web servers, a few database servers. Basically, we've taken what was once a monolithic application and split it into atomic components that are replaceable.

In the cloud though, we don't have to stop there. The above is a "bare metal servers on a cloud" mentality. What you should strive for is a true modular application that not only is broken up into smaller pieces, but also consumes services.

(arch of cloud app with services goes here)

This demonstrates a departure from only consuming an infrastructure service to consuming a mixture of IaaS and Platform services. Why build your own payment system when you can use a great company like Stripe? Why maintain your own storage platform when you can consume Cloud Files via an API? Why run your own email server when you can consume Mailgun? Compute power is expensive: you should only use it for tasks that require compute power!


