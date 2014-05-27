---
layout: post
published: true
title: Using AndroVM
category: programming
tags: android mobile
comments: []
---

Update: the team behind AndroVM have released [Genymotion](http://www.genymotion.com/ "Genymotion"). It greatly simplifies the downloading of device images and automatically connects them to adb.

[AndroVM](http://androvm.org/) is a novel alternative to the Android emulator with Android running in a virtual machine (VM) using VirtualBox. It works quite well and for some requirements can be significantly faster than the Android emulator.

Installing and setting up is as simple as installing VirtualBox and importing the Android VirtualBox images available [here](http://androvm.org/blog/download/)

To debug and install your apps using adb:

1. Get the IP address of the VM from the AndroVM Config app running in the VM. If the app doesn't show an IP address make sure you've created a host only network in the VirtualBox preferences and configured the VM to use that host only adapter as "Adapter 1".
2. Execute 'adb connect <IP address>'
3. From this point the standard adb commands (such as 'adb install') should work

AndroVM is still in it's infancy and as such does have a few bigger limitations:

- Only Android 4.1 images are available.
- The available screen resolutions are limited but can be easily selected using the AndroVM Config app running in the VM.

On the plus side, the team behind AndroVM have promised configuration management software to make it easier to create and run different Android configurations. Additionally the software will also automatically connect the VM to adb.
