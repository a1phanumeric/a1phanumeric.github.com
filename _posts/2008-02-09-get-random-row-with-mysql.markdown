---
author: admin
comments: true
date: 2008-02-09 00:51:31
layout: post
slug: get-random-row-with-mysql
title: Get Random Row with MySQL
wordpress_id: 14
categories:
- mysql
tags:
- fast
- mysql
- random row
---

**UPDATE:** Please see my newer atricle on [how to retrieve a random row, faster, without RAND()]({{ site.url }}/mysql/get-random-row-with-mysql-without-order-by-rand/).

This post assumes you know how to create and use a connection to a MySQL database in PHP and have a table named 'quotes' as shown below. In this post, I will aim to teach you how to use PHP to pull random quotes from a MYSQL table of quotes. This can be easily extended to pull a random banner as will be explained at the end of the post.

Let's firstly assume we have a MySQL table similar to the following:

{% highlight text %}
+----+------------------------------------------------+
| id | quote                                          |
+----+------------------------------------------------+
| 1  | I know Karate... and many other Chinese words! |
+----+------------------------------------------------+
| 2  | w00t this is geeky                             |
+----+------------------------------------------------+
| 3  | You're CLINICALLY MENTAL!                      |
+----+------------------------------------------------+
{% endhighlight %}

Now, in your PHP code, we need to:

1. Build a suitable MySQL query to obtain a random result from the 'quotes' table.
2. Store the result of the query to be used in the HTML somewhere.
3. Output the result in the HTML somewhere.

So, for step one we'd use something like the following:

{% highlight php %}
<?php
$sSQLQuery = "SELECT quote FROM quotes ORDER BY RAND() LIMIT 1";
$aResult = mysql_query($sSQLQuery);
$aRow = mysql_fetch_array($aResult, MYSQL_ASSOC);
$sQuoteOfTheDay = $aRow['quote'];
?>
{% endhighlight %}

There, the variable `$sQuoteOfTheDay` now has the value of our randomly pulled quote. Let's just analyse each line of the code above to see what it does.

{% highlight php %}
<?php
$sSQLQuery = "SELECT quote FROM quotes ORDER BY RAND() LIMIT 1";
?>
{% endhighlight %}

This line stores the MySQL query we're going to use against the database. It says, in laymans terms, "Select just one random value of the quote field from the table named quotes". All this line does though is store the query into the variable `$sSQLQuery`.

{% highlight php %}
<?php
$aResult = mysql_query($sSQLQuery);
?>
{% endhighlight %}

This line runs the MySQL query, storing the result of running the query in the variable '$aResult'. It's important that we store the result of the mysql_query in a variable, as the result of running a successful MySQL query using PHP's function 'mysql_query' doesn’t return a nicely formatted array that we can necessarily use.

{% highlight php %}
<?php
$aRow = mysql_fetch_assoc($aResult);
?>
{% endhighlight %}

This is probably the hardest line for me to explain. Firstly, many of you may have seen a similar line like this used in a 'while' loop. However, our MySQL query used the 'LIMIT 1' string, so we know we’re only going to get ONE result, hence no need for a loop. The function 'mysql_fetch_assoc' takes one parameter: the result of the successful 'mysql_query' which as we know is `$aResult`. My biggest tip here is to use the 'assoc' method wherever possible, as it creates the array in such a way that we can reference each element by the column name, not a number. This is particularly useful if you ever update the MySQL table to have more columns.

Anyway, this line basically says 'Fill the variable `$aRow` with the CURRENT row of the returned query'. We know that the CURRENT row of the returned query is the ONLY row, hence (again) the lack of a loop. As the result of our query would return something like:

{% highlight text %}
+-----------------------------------------------+
| quote                                         |
+-----------------------------------------------+
| You're CLINICALLY MENTAL!                     |
+-----------------------------------------------+
{% endhighlight %}

Our variable (or array) would literally look something like:

{% highlight bash %}
Array ( "quote" = "You're CLINICALLY MENTAL!" )
{% endhighlight %}

Which leads us on to our last line:

{% highlight php %}
<?php
$sQuoteOfTheDay = $aRow['quote'];
?>
{% endhighlight %}

Which just assigns the value of `$aRow['quote']` to the variable `$sQuoteOfTheDay`. In other words, `$sQuoteOfTheDay` now has the value of a random quote pulled from the database of quotes.

To use this in an HTML page, we would simply just use this (AFTER the above code has grabbed the random quote from the DB for us):

{% highlight php %}
<?php
echo $sQuoteOfTheDay;
?>
{% endhighlight %}

Which will output the quote of the day somewhere in the HTML code.

As I said at the beginning, this can be extended easily to pull an image for a banner by simply changing the quotes table to store image paths such as 'images/my_image.png' which can then be pulled in the same way, and then output similar to the following:

{% highlight html %}
![My Image](<?php print $sImageOfTheDay; ?>)
{% endhighlight %}

Obviously we changed the variable name here to `$sImageOfTheDay` just to keep things constant.

Hope this has helped someone!
