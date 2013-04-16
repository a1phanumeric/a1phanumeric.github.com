---
author: admin
comments: true
date: 2010-08-17 20:50:35
layout: post
slug: simple-php-mysql-class
title: Simple PHP MySQL Class
wordpress_id: 32
categories:
- php
tags:
- class
- mysql
- php
---

That's right! I have a simple MySQL class file that you can use in your PHP projects. I've been using it for years, and it's never let me down!

You can grab it on my Github: [http://github.com/a1phanumeric/PHP-MySQL-Class](http://github.com/a1phanumeric/PHP-MySQL-Class).

## Setup

The setup is simple:

Simply include this class into your project like so:

{% highlight php %}
<?php
include_once('/path/to/class.MySQL.php');
?>
{% endhighlight %}

Then invoke the class in your project using the class constructor (which now sets the db credentials):

- `$oMySQL = new MySQL(MYSQL_NAME, MYSQL_USER, MYSQL_PASS, [MYSQL_HOST]);`
- `MYSQL_NAME` The name of your database
- `MYSQL_USER` Your username for the server / database
- `MYSQL_PASS` Your password for the server / database
- `MYSQL_HOST` The hostname of the MySQL server (*optional*, defaults to 'localhost')


## Usage

To use this class, you'd first init the object like so (using example credentials):

`$oMySQL = new MySQL('my_database','username','password');`

Provided you see no errors, you are now connected and can execute full MySQL queries using:

`$oMySQL->ExecuteSQL($query);`

`ExecuteSQL()` will return an array of results, or a true (if an UPDATE or DELETE).

There are other functions such as `Insert()`, `Delete()` and `Select()` which may or may not help with your queries to the database.

## Example

To show you how easy this class is to use, consider you have a table called *admin*, which contains the following:

{% highlight text %}
+----+--------------+
| id | username     |
+----+--------------+
|  1 | superuser    |
|  2 | a1phanumeric |
+----+--------------+
{% endhighlight %}

To add a user, you'd simply use:

{% highlight php %}
<?php
$newUser = array('username' => 'Thrackhamator');
$oMySQL->Insert($newUser, 'admin');
?>
{% endhighlight %}

And voila:

{% highlight text %}
+----+---------------+
| id | username      |
+----+---------------+
|  1 | superuser     |
|  2 | a1phanumeric  |
|  3 | Thrackhamator |
+----+---------------+
{% endhighlight %}

To get the results into a usable array, just use `$oMySQL->Select('admin')` ...for example, doing the following:

`print_r($oMySQL->Select('admin'));`

will yield:

{% highlight text %}
Array
(
    [0] => Array
        (
            [id] => 1
            [username] => superuser
        )

    [1] => Array
        (
            [id] => 2
            [username] => a1phanumeric
        )

    [2] => Array
        (
            [id] => 3
            [username] => Thrackhamator
        )

)
{% endhighlight %}

So have a play and let me know what you think ...or fork me!

The code is all here:

[http://github.com/a1phanumeric/PHP-MySQL-Class](http://github.com/a1phanumeric/PHP-MySQL-Class)
