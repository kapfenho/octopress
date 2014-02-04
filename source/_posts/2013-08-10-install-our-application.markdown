---
layout: post
title: "Install our application"
date: 2013-08-10 15:14
comments: true
categories: Internet, Mac
---

> Do you like to install needless and gratuitous software on your machine?
> No...?
> Even not when it constantly shows you ads and perhaps sends data to
> other servers...?

    note "?db???..." 2

No, still not. I don't understand why I should install an application for
using 3rd party USB modems to connect via UMTS.

    ! ---- Hang up and reset DCE ----

Still it was not easy to find "drivers" for the common ZTE USB modems,
used in several European countries.

    write "ATZ\13"

After some investigation it seems quite easy to implement: all you need is 
a modem script that talks properly to the device. Then you can use standard 
OS features for connecting, etc.

[Modem Scripts at Github](https://github.com/kapfenho/ccl-modem-scripts)


