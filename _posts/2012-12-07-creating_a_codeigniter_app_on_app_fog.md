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

[AppFog](http://www.appfog.com) is a PaaS (platform as a service) provider.  They created [PHP Fog](http://www.phpfog.com) which will be discontinued on December 21, 2012.  AppFog is a great solution for those who are uncomfortable with managing their own VPS and all that it entails.  Many developers start off using low-cost shared hosting solutions that require no dev ops knowledge or server admin chops.

There does come a time when most developers grow out of these services and need to explore a VPS or dedicated server.  These are usually more expensive depending on the RAM you get.  Wouldn't you love it if you had the simplicity of shared hosting, but also the benifits of a VPS and still retain most of the customizations you'd have with a VPS?  That's where these PaaS providers step in.  I have yet to look into some of AppFog's competitors like [Engine Yard](http://www.engineyard.com) and [pagoda box](http://pagodabox.com), but I'm sure they too offer similar pricing and solid up-time.

AppFog is my choice for now simply because I used their PHP Fog service for a bit and their [roadmap for upcoming features](https://docs.appfog.com/roadmap) looks pretty inticing.

PHP Fog offered a quick start for Codeigniter applications, however AppFog is currently not offering a clean install of Codeigniter, so you'll have to start with an empty PHP app and upload Codeigniter yourself.

# Create a PHP App

<a href="/img/appfog01.png"><img src="/img/appfog01.png" /></a>

{% highlight php %}
<?php
// application/config/database.php
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

{% highlight php %}
<?php
// application/config/database.php
$services_json = json_decode(getenv("VCAP_SERVICES"),true);
$mysql_config = $services_json["mysql-5.1"][0]["credentials"];

$db['default']['hostname'] = $mysql_config['hostname'];
$db['default']['username'] = $mysql_config['user'];
$db['default']['password'] = $mysql_config['password'];
$db['default']['database'] = $mysql_config['name'];
$db['default']['port']     = $mysql_config['port'];
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

{% highlight php %}
<?php
// application/config/autoload.php
$autoload['libraries'] = array('database');
?>
{% endhighlight %}