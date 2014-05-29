---
layout: post
published: true
title: Android screen utility class
category: programming
tags: android mobile
comments: []
---

There are a few common operations I infrequently perform related to the Android screen:

- Converting a pixels value to the corresponding density independent pixels (DIP) value and vice versa
- Getting the screen dimensions
- Checking what orientation the device is in
- Getting the screen size (small, normal, large, extra-large)

They're not difficult but it is annoying having to remember exactly how they're done so I created a utility class to make it a bit easier. Here it is:

<script type="text/javascript" src="https://gist.github.com/2504204.js?file=ScreenUtils.java"></script>
