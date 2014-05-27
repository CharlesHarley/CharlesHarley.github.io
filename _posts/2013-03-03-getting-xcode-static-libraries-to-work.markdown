---
layout: post
published: true
title: Getting Xcode static libraries to work
excerpt: Getting Xcode static library dependencies to work is way harder than it should
  be! This weekend I setup a new Xcode OS X workspace with 2 child projects. One project
  was the app and the other a static library used by the app. Setting the static library
  as a dependency of the app project took ages. Part of the reason was my lack of
  knowledge but Xcode should really give a bit more help.
category: programming
tags: xcode
comments: []
---

Getting Xcode static library dependencies to work is way harder than it should be! This weekend I setup a new Xcode OS X workspace with 2 child projects. One project was the app and the other a static library used by the app. Setting the static library as a dependency of the app project took ages. Part of the reason was my lack of knowledge but Xcode should really give a bit more help.

Anyway, for the good of the people here are 2 blog posts I found helpful. I was using Xcode 4.6.

[http://blog.stevex.net/2012/04/static-libraries-in-xcode](http://blog.stevex.net/2012/04/static-libraries-in-xcode) - I found this the most useful for setting up the dependency, however steps 10 to 16 weren't needed for Xcode 4.6. Probably because Apple fixed a bug.

[http://blog.carbonfive.com/2011/04/04/using-open-source-static-libraries-in-xcode-4](http://blog.carbonfive.com/2011/04/04/using-open-source-static-libraries-in-xcode-4) - The instructions for setting up a dependency didn't quite work for some reason. However, the section on creating a static library is useful.
