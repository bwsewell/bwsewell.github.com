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
title: Namespacing Models in Laravel 3
---

Let me preface this post by saying I'm new to Laravel and I'm not an expert on the topic of namespacing.  It's a relatively new concept for me.  I've been working with PHP for a long time, but have been using Ruby for the past year and a half for my job, so I'm somewhat familiar with it.

I've been writing an app using [Laravel](http://laravel.com) recently.  I was an avid user of [Codeigniter](http://codeigniter.com) before, but I have since learned to enjoy Laravel much more.

## Resolving Model Name Conflicts with Laravel Classes

When developing my app, I ran into an issue where I had a model named `Filter` which would conflict with Laravel's `Routing\Filter` class.  I couldn't figure out how to fix it, so I just renamed my model to `Filtr` which became very annoying.

After some helpful peeps over on the Laravel IRC channel pointed me in the right direction as well as some examples on the forums, I figured out what this whole namespacing thing was.

Here's what my model looked like before:

{% highlight php %}
<?php
  // application/models/filter.php

  class Filter extends Eloquent {
    // model stuff goes here
  }
?>
{% endhighlight %}

Whenever I attempted to use this model, I'd receive this error from Laravel:

`Call to undefined method Laravel\Routing\Filter::connection()`

This is because it's assuming my reference to `Filter` is in the `Laravel\Routing` namespace.

In order to make this model work, I need to put it within a `Models` namespace:

{% highlight php %}
<?php
  // application/models/filter.php

  namespace Models;

  class Filter extends \Eloquent {
    // model stuff goes here
  }
?>
{% endhighlight %}

In order to make this work properly, you'll also have to add some code to your `application/start.php` file:

{% highlight php %}
<?php
  // application/start.php

  Autoloader::directories(array(
    // path('app').'models',   <-- this line needs to be commented out
    path('app').'libraries',
  ));

  Autoloader::namespaces(array(
      'Models' => __DIR__ . DS . 'models',
  ));
?>
{% endhighlight %}

You'll notice I commented out `path('app').'models`.  This is to prevent Laravel from auto-loading my models without the  `Models` namespace... I know confusing right?

So now if I try to utilize this model somewhere inside a Route, Controller or View, I need to access it like this:

{% highlight php %}
<?php
  // Get all filter objects
  $filters = Models\Filter::all();
?>
{% endhighlight %}

Instead of this:

{% highlight php %}
<?php
  // Get all filter objects
  $filters = Filter::all();
?>
{% endhighlight %}

I know that seems cumbersome, but you'll get used to it.

## What about Eloquent relationships?

Let's say I have another model, "Category" that has many Filters.  Here's how we set that relationship up inside the Models namespace:

{% highlight php %}
<?php
  // application/models/category.php

  namespace Models;

  class Category extends \Eloquent {

    public function filters() {
      return $this->has_many('Models\Filter');
    }

  }
?>
{% endhighlight %}

Now this will still work the way it's supposed to:

{% highlight php %}
<?php
  $category = Models\Category::find(1);
  $category_filters = $category->filters();
?>
{% endhighlight %}

## Referencing namespaced models inside of models

Let's say we want to write a function to get me all the Filters that are flagged as 'active' in the database.  I'm just pretending we have a schema set up like this:

**filters:**
{% highlight console %}
id     - INTEGER
 name   - VARCHAR
 active - TINYINT
{% endhighlight %}

Let's write a function inside our Filter model to return all filters with 'active' set to 1

{% highlight php %}
<?php
  // application/models/filter.php

  namespace Models;

  class Filter extends \Eloquent {

    /* Returns active Filters */
    public static function active() {
      return Filter::where_active(1)->order_by('name','asc')->get();
    }

  }
?>
{% endhighlight %}

Notice we did not prefix Filter with `Models\`.  The reason we don't have to is because we are already inside the Models namespace.