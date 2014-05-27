---
layout: post
published: true
title: BugSense setup on Android
category: programming
tags: android mobile bugsense
---

We started using [BugSense](http://www.bugsense.com) at work recently to log uncaught exceptions in our Android applications. It works really well and is very easy to setup, best of all it's free!

The setup instructions say to call `BugSenseHandler.setup()` in your "main/first activity" but we found that calling this method in the subclassed `Application.onCreate()` method also works and is better if your application has more than one entry point.

The complete setup instructions can be found [here](http://www.bugsense.com/docs/android "BugSense Android documentation").
