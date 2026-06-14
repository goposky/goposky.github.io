---
layout: post
title: "Nodejs API development impressions"
date: 2016-05-29 10:00:00 +0000
image: /assets/images/nodejs.jpg
tags: [nodejs, angularjs, javascript, rest, swagger]
excerpt: "Recently at work I built an API+Portal solution with nodejs. Nodejs is a great way to develop api's very fast. A great place to pick up nodejs basics is here. For someone starting to develop a nodejs application for the enterprise there are a few considerations which I list"
---

Recently at work I built an API+Portal solution with nodejs. Nodejs is a great way to develop api's very fast. A great place to pick up nodejs basics is [here](https://github.com/maxogden/art-of-node). For someone starting to develop a nodejs application for the enterprise there are a few considerations which I list below. This is not an in depth how-to document or study of design strategies but a summary of a few technologies and methods involved. For design best practices I would highly recommend the guideline known as the [Twelve-Factor app](http://12factor.net/).

###### Framework selection

A nice selection of node frameworks can be found [here](http://nodeframework.com/). For my application I used the [restify](http://restify.com/) module which provides a framework to build 'correct REST webservices'. In other words, it provides for building pure rest api implementation unlike express, which is more of an MVC framework. With resitfy I could decouple the UI and API into separate applications. Node modules, which are basically libraries are available for all kinds of needs.

###### Angularjs

Angularjs is excellent when it comes to simplifying frontend development. Databinding works like magic, and so are the other innumerous features. The [Flatlander store tutorial](http://angular.codeschool.com/) is a good place to pick up the essentials. Together with [Bootstrap](http://getbootstrap.com/) you can build pretty and snappy websites easily.

###### Know your javascript

Of particular importance is Callbacks. Everything in nodejs is asynchronous, ie., the execution carries on without waiting for the results of the current line of code. Callbacks allow you to capture the results of high-response-time functions (like DB actions, LDAP queries, etc) and trigger actions accordingly. Once you get the hang of hit, you will sooner or later end up with something known as [Callback hell](http://callbackhell.com/). And then there is the problem of [callbacks within loops](http://tobyho.com/2011/11/02/callbacks-in-loops). Callbacks are both the boon and bane of javascript. There are better ways to mitigate callback hell, such as with the use of [Promises](https://www.promisejs.org/), Generators (only in JS 6), or other frameworks like [Rxjs](http://reactivex.io/) which is based on the Observer pattern.

###### Application Structure

Structuring the application is an important first step. Best practice is to break down the application into feature modules (subdirectories), and further breakdown each module into controllers, models, views, factories, etc. The wiring can be done via routes, and the various frameworks provides ways to do this. This way of modularizing helps with maintenance as the codebase grows. Recommended practice is to add code in layers as wrappers of more specific implementation.

###### API design

Modeling the application around logical resources and then using HTTP methods to operate on these resources is the essence RESTful API design. This topic deems much consideration and there is plenty of literature and best practices available.

###### API discovery/documentation

The use of tools like [Swagger](http://swagger.io/) allow you to both define your APIs, and automatically generate the stubs, thus letting you focus just on writing the actual functional code. Swagger also generates the API documentation. It is advisable to implement a solution like swagger early on since retroactive fitting can give you a headache.

###### Authentication & Authorization

If your application consists of stateless API which require some form of user authentication I recommend using [JSON Web tokens](https://jwt.io/introduction). JWT is an authentication protocol and using jsonwebtoken we can limit user access to the application. A more comprehensive A&A solution can be implemented using [oath2](http://oauth.net/2) which is an authentication framework.

###### Backend

I used the [node mongodb driver](https://www.npmjs.com/package/mongodb) to connect to my mongo database. Mongoose is another useful mongodb module which many use. My application also required to interface with LDAP for which I used the [ldapjs](http://ldapjs.org/) module. As mentioned before there are modules for all kinds of needs and the large node community makes it easy to find solutions on the web.

###### Test-driven development

TDD is an important agile development principle and Nodejs has built-in assert module to support this. Using a test framework like [Mocha](https://mochajs.org/) in conjunction with nodejs allows developers to write better code.

###### Continuous delivery

Last but not the least, it is important to plug in any application development exercise to a continuous delivery setup. This includes your usual suspects: source code repo, a dockerized application build and deployment setup, automated testing, etc. Again this topic deserves its own blog post.
