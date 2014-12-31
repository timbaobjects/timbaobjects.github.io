---
layout: post
permalink: /blog/2012/02/two-are-better-than-one-when-it-comes-to-servers/
title: Two are better than one when it comes to servers
date: 2012-02-21 18:41:49+00:00
excerpt: "Two and probably more are better than one when it comes to deploying servers for a web application. When we deploy our applications, we generally like..."

author:
  name: Tim Akinbo
  twitter: takinbo
  gplus: takinbo 
  bio: Founder
  image: takinbo.jpeg
  wordpress_id: 196
  comments: true
categories:
- Software Development
---

Two (and probably more) are better than one when it comes to deploying servers for a web application. When we deploy our applications, we generally like to separate the database server from the application server. If we were deploying a PHP web application for instance, the web server (which also doubles as the application server in the simplest cases) will be deployed separately from the MySQL database server. That is going to cost more in terms of hardware resources but we've found out that it not only improves the application performance but it also increases the application reliability. At TimbaObjects, we obsess over the visual appeal, speed, data integrity and reliability of our applications and so our approach to building such applications generally means we separating data processing from data storage.

In early hours of this morning, one of our application servers which is being used to store, manage and process data for one of our clients went down. Our client is an accredited elections observation group and they recruit and train their election observers to fill out and send answers to an elections checklist (using structured text messages) to a shortcode which is then routed to an application we built that parses, validates and stores the data in this structured text message. It then sends out a response as a reply to the sender.

Our connectivity with the telecom operators is via an [SMPP](http://en.wikipedia.org/wiki/SMPP) link with [our content aggregators](http://www.3wc4life.net/) and that's a good thing because if our server goes down for any reason, messages bound for our shortcode get aggregated until we re-establish the connection. This is about the second time we've had this problem in the close-to-one-year period this application has been live. Further investigations into the crash revealed that we ran out of memory on the box and while the operating system was attempting to salvage more memory by killing tasks, it seg-faulted. Unfortunately, our watchdog is unable to detect this condition and reboot the server so a reboot had to be done manually. This was an occurrence only happening on the application server; the database server has been largely unscathed - we have an uptime of over 303 days on the database server (as at the writing of this post).

Our deployment uses an Apache server with [mod_wsgi](http://code.google.com/p/modwsgi/) to serve the application and preforks 10 processes to serve requests. In order to provide access to previous elections data, we copy over this configuration for every election. With a current deployment covering five elections schedules, we have over 50 apache processes. With a server deployed with only 768MB of RAM, that's a lot of memory we're talking about here. The machine on which our web server is deployed also doubles as the SMS processing server. So we have an additional [Kannel](http://kannel.org/) bearerbox and smsbox processes routing messages over HTTP to a node.js HTTP proxy server we built to handle HTTP requests on behalf of [RapidSMS](http://www.rapidsms.org/) (which our application is built on). Add all this processes together and you have a box that's about to go belly up.

We solved the problem by reducing the number of processes apache forks to handle requests for the archived applications since those applications don't get used that much. From about 59 apache processes, it went down to 25. We're currently using 56% of available RAM now to serve the applications and no swap yet. With a swap size of only 256MB, if we had something much bigger, we probably wouldn't have run out of memory.

Of course we could have just used one server with a lot of RAM to serve both database and application servers, however, a lot of things can go wrong and the fewer applications you have running on a server, the better for its stability. You simply don't want to have a single point of failure in your application architecture.

Next time, we're considering separating the SMS processing application from the application server so in the event that the application server goes down, text messages will continue being processed.
