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
tags: rubymotion
-
title: Change User Agent in UIWebView in RubyMotion
---

As I've been going along learning how to do different things in RubyMotion, I figure I should share the knowledge when I make little discoveries here and there.  Sometimes, I don't understand how to convert something from objective-c to ruby and it drives me nuts. So here's the first little code snippet I'd like to share:

I've typically added this line to the `app_delegate.rb` file inside `application(application, didFinishLaunchingWithOptions:launchOptions)`

{% highlight ruby %}
# Modify UserAgent
NSUserDefaults.standardUserDefaults.registerDefaults({UserAgent: "my-custom-user-agent-name"})
{% endhighlight %}

I've been using this trick to help hide certain html elements in a webView depending on whether or not the user is looking at a webpage in safari or within the app itself.