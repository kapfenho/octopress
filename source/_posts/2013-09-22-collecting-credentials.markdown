---
layout: post
title: "Collecting Credentials"
date: 2013-09-22 13:07
comments: true
categories: Security, IAM
---

> Sorry, the password you tried is already being used by Dorthy, please try something else.
> -- http://nadh.in/docs/geek_jokes/

Login pages with user name and password are not the preferred
authentication method in enterprises. Still we need one, at least as a
fall back solution or when clients or a certain deployment method
demand it.

If you are using Oracle Access Manager (formerly know as Oblix CoreID, NetPoint) 
you probably know that it my be easier to find water in the dessert than a good piece of 
documentation for that application.

Therefore we will post some solutions we've built or collected working
on OAM projects.

[Here are login pages](https://github.com/kapfenho/ssocc) that you can use 
in DCC or ECC mode. It's important to mention the login page needs to 
implement the GET and POST methods.  It will be called with a POST by 
WebGate (not by the browser) when the user enters an invalid password.

The pages can be deployed e.g. in the DMZ as a stand alone application
(DCC) or with the access server on the same J2EE container like WebLogic 
(ECC).

You can also extend it to be used as a more friendly user interface for 
Identity Manager transactions. We've used that approach for password pages 
and other OIM self service areas when you don't want to use the more heavy 
Fusion pages.


