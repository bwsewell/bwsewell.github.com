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
title: Creating a Codeigniter App on AppFog
---

[AppFog](http://www.appfog.com) is a [PaaS (platform as a service)](http://en.wikipedia.org/wiki/Platform_as_a_service) provider.  They created [PHP Fog](http://www.phpfog.com) which will be discontinued on December 21, 2012.  AppFog is a great solution for those who are uncomfortable with managing their own VPS and all that it entails.  Many developers start off using low-cost shared hosting solutions that require no dev ops knowledge or server admin chops.

There does come a time when most developers grow out of these services and need to explore a VPS or dedicated server.  These are usually more expensive depending on the RAM you get.  Wouldn't you love it if you had the simplicity of shared hosting, but also the benifits of a VPS and still retain most of the customizations you'd have with a VPS?  That's where these PaaS providers step in.  I have yet to look into some of AppFog's competitors like [Engine Yard](http://www.engineyard.com) and [pagoda box](http://pagodabox.com), but I'm sure they too offer similar pricing and solid up-time.

AppFog is my choice for now simply because I used their PHP Fog service for a bit and their [roadmap for upcoming features](https://docs.appfog.com/roadmap) looks pretty enticing.

PHP Fog offered a quick start for Codeigniter applications, however AppFog is currently not offering a clean install of Codeigniter, so you'll have to start with an empty PHP app and upload Codeigniter yourself.

## 1. Create a PHP App

[Create an AppFog account](https://console.appfog.com/signup).

Once logged in you will be prompted to create your first app.  Select the **PHP** application.

<a href="/img/appfog01.png"><img class="fit" src="/img/appfog01.png" /></a>

The next step is to select your prefferred infrastructure.  I will be using **AWS US East** for purposes of this tutorial.

Give your app a name and click **Create App**.

<a href="/img/appfog02.png"><img class="fit" src="/img/appfog02.png" /></a>

Once your app is built, you will be redirected to your app's dashboard:

<a href="/img/appfog04.png"><img class="fit" src="/img/appfog04.png" /></a>

## 2. Install AppFog's Command Line Tool

AppFog offers a handy command line tool to create, update and manage your hosted apps, as well as tunneling into services they offer like MySQL, Redis, MongoDB, PostgreSQL and RabbitMQ.  PHP Fog, the first implementation of AppFog had used git and git deployments originally to manage apps, however they recognized that not every developer uses git for source control.

The installation instructions for the AppFog command line tool is pretty [straight-forward](https://docs.appfog.com/getting-started/af-cli).  All you have to do is install the af gem.  This gem does require that you have Ruby 1.8.7 or newer installed on your machine.

{% highlight console %}
$ gem install af
{% endhighlight %}

## 3. Create a local version of your app

{% highlight console %}
$ mkdir ci-test-app
$ cd ci-test-app
{% endhighlight %}

[Download a fresh Codeigniter install](http://ellislab.com/codeigniter/download) into the folder containing your app.

## 4. Upload your Codeiginiter install to AppFog

Now that you have a clean Codeigniter install on your machine, it's time to upload it to your AppFog app using the af tool.

Run the following command inside your app folder on your local machine:

{% highlight console %}
$ af update ci-test-app
{% endhighlight %}
Where **ci-test-app** is the name of your app.

You should receive feedback that looks like this:

{% highlight console %}
$ af update ci-test-app
$ Uploading Application:
$   Checking for available resources: OK
$   Processing resources: OK
$   Packing application: OK
$   Uploading (6K): OK
$ Push Status: OK
$ Stopping Application 'ci-test-app': OK
$ Staging Application 'ci-test-app': OK
$ Starting Application 'ci-test-app': OK
{% endhighlight %}

If everything goes well, you should have successfully pushed a Codeigniter install to your AppFog app.

You can check to see if everyone uploaded correctly by clicking on **Visit Live Site** on your app's dashboard.

<a href="/img/appfog04_2.png"><img class="fit" src="/img/appfog04_2.png" /></a>

Hopefully you'll see this...

<a href="/img/appfog10.png"><img class="fit" src="/img/appfog10.png" /></a>

## 5. Provision your MySQL database

I'm going to use MySQL for this tutorial... if you prefer Mongo, PostreSQL or Redis, the following steps will be relatively similar.

On your app's dashboard, click on the **Services** tab on the side.  From here, you can create your MySQL database.

<a href="/img/appfog05.png"><img class="fit" src="/img/appfog05.png" /></a>

Select **MySQL** from the services list and give it a name.  I named mine "ci-test-app-mysql".

Click **Create** and wait for your db to bind and provision.

## 6. Connect Codeigniter to your MySQL database

Now that your MySQL database has been created, you need to add the proper credentials to your database config file in your CI install.

Open your **application/config/database.php** file and locate this section at the bottom:

{% highlight php %}
<?php
$db['default']['hostname'] = 'localhost';
$db['default']['username'] = '';
$db['default']['password'] = '';
$db['default']['database'] = '';
$db['default']['dbdriver'] = 'mysql';
$db['default']['dbprefix'] = '';
$db['default']['pconnect'] = TRUE;
$db['default']['db_debug'] = TRUE;
$db['default']['cache_on'] = FALSE;
$db['default']['cachedir'] = '';
$db['default']['char_set'] = 'utf8';
$db['default']['dbcollat'] = 'utf8_general_ci';
$db['default']['swap_pre'] = '';
$db['default']['autoinit'] = TRUE;
$db['default']['stricton'] = FALSE;
?>
{% endhighlight %}

You will need to add the following code above this block

{% highlight php %}
<?php
$services_json = json_decode(getenv("VCAP_SERVICES"),true);
$mysql_config = $services_json["mysql-5.1"][0]["credentials"];
?>
{% endhighlight %}

Then replaces the first four lines with these:

{% highlight php %}
<?php
// $db['default']['hostname'] = 'localhost';
// $db['default']['username'] = '';
// $db['default']['password'] = '';
// $db['default']['database'] = '';
$db['default']['hostname'] = $mysql_config['hostname'];
$db['default']['username'] = $mysql_config['user'];
$db['default']['password'] = $mysql_config['password'];
$db['default']['database'] = $mysql_config['name'];
$db['default']['port']     = $mysql_config['port'];
?>
{% endhighlight %}

The reason you need to do this is because AppFog stores all credentials for your bound services inside an environment variable called `VCAP_SERVICES`.  This data is formatted as a JSON document.  You can read more about it [here](https://docs.appfog.com/services).

Ok, so now all you have to do is autoload the database library in Codeigniter so that your CI install will attempt to connect to your database.

Edit the line in **application/config/autoload.php** as follows:

{% highlight php %}
<?php
$autoload['libraries'] = array('database');
?>
{% endhighlight %}

Go back to your app dashboard and visit your live site.  If you still see the same default CI page, then you've successfully connected your MySQL database!

<a name="phpmyadmin"></a>
## 7. Setup phpMyAdmin

AppFog treats phpMyAdmin as it's own app, similar to how it treats your Codeigniter Install.  Aside from tunnelling into your MySQL database, phpMyAdmin will be your best way to interface with your database for now.

Goto your dashboard and click on **Create App** in the top navigation

Select phpMyAdmin from the application list

<a href="/img/appfog11.png"><img class="fit" src="/img/appfog11.png" /></a>

Same as you did for your PHP app, select your preferred infrastructure... once again I'm using AWS US East.  Then name your phpMyAdmin install... I just named mine "ci-test-app-mysql".

<a href="/img/appfog12.png"><img class="fit" src="/img/appfog12.png" /></a>

Once your app is created, from your dashboard click on **Services** on the side.  You will see your MySQL database you created earlier.  Click the **Bind** button to bind this service to your phpMyAdmin installation.

<a href="/img/appfog13.png"><img class="fit" src="/img/appfog13.png" /></a>

You'll also need to add a password to your phpMyAdmin app. Click on the **Env Variables** tab on the left. Create an environment variable called `PMA_PASSWORD` and set it to whatever password you want.

You can set the value to whatever you want as your phpMyAdmin password. Your default phpMyAdmin username is the email address your AppFog account is under, but you can optionally override that to something of your choice by creating another environment variable called `PMA_USERNAME`.

As an example I've set both my username and password to "root".  I do not recommend using these values at all by the way.

<a href="/img/appfog14.png"><img class="fit" src="/img/appfog14.png" /></a>

So now you will be able to login to your phpMyAdmin installation to manage your database by clicking on the **Visit Live Site** link on your phpMyAdmin app dashboard.

<a href="/img/appfog15.png"><img class="fit" src="/img/appfog15.png" /></a>

<a href="/img/appfog16.png"><img class="fit" src="/img/appfog16.png" /></a>

## You're all set!

Enjoy using AppFog to host your Codeigniter app.