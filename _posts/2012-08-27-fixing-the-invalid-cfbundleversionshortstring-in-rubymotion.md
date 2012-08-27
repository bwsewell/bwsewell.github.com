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
title: Fixing the Invalid CFBundleVersionShortString in RubyMotion
---

If you've gotten as far as submitting an app through xCode, getting stuck with this error message can be confusing.

> This bundle is invalid.  The key CFBundleVersionShortString in the Info.plist file must contain a higher version than that of the previously uploaded version.

In a RubyMotion project, the *Info.plist* file is derived from the configuration object exposed in the `Rakefile` file. For example, the `CFBundleShortVersionString` variable in Info.plist is derived from the name variable in the Rakefile. Most of the time, the configuration object will let you control the necessary settings of his project.

All you have to do is set the value for `CFBundleShortVersionString` in your `Rakefile` file to what you've set your `app.version` to:

{% highlight ruby %}
Motion::Project::App.setup do |app|
  # ...
  app.version = '1.1'
  app.info_plist['CFBundleShortVersionString'] = '1.1'
end
{% endhighlight %}

**NOTE:** Also, as of June 1, 2012, RubyMotion informed us that the `app.info_plist` call will cause all following calls in your Rakefile to be ignored, so make sure you put this at the very end of your `Rakefile` file.  Known issue, I'm sure they'll get that fixed.