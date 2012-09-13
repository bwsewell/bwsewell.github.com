---
author: Brian Sewell
excerpt:
image:
  url:
  title:
  alt:
  attribution:
layout: post
published: true
tags:
-
title: Supporting the iPhone 5 (4-inch) screen in RubyMotion 
---

After installing the GM release of xCode 4.5 and the iOS 6 SDK seed I wanted to see if building an app I've been working on in RubyMotion would fill the entire 4" display in the simulator.  No luck the first time around.  I got the letter-boxed version to show up.  After a short time searching the dev forum on Apple's iOS developer site, I found this solution....

> Create a launch image that is 640 x 1136 pixels

All you have to do is add the 4" launch image to the `/resources` folder of your project.  The file should be named `Default-568h@2x.png` or `Default-568h@2x~iphone.png` if it's a universal app.  Since the iPhone 5 has no non-retina counterpart, you obviously won't need the non-@2x version.

BOOM!