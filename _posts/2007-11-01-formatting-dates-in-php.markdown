---
author: admin
comments: true
date: 2007-11-01 21:56:57
layout: post
slug: formatting-dates-in-php
title: Formatting dates in PHP
wordpress_id: 3
categories:
- php
tags:
- code
- php
---

Ok, so how do you **format dates in PHP** so it outputs the date format you want? Well thanks to PHP’s [date()](http://uk.php.net/date) and [strtotime()](http://uk2.php.net/strtotime) function, we can do all that! To kick off, lets take the most common date format ‘**YYYY-MM-DD HH:II:SS**‘. This date format seems to be most favoured as it increments in such a way that allows you to query a database that has multiple records in a useful way, such as:

{% highlight mysql %}
SELECT * FROM dates WHERE date > '2006-03-05 11:00:00';
{% endhighlight %}

Whereas if you were to use a different date format, such as ‘**DD-MM-YYYY HH:II:SS**‘ the incrementation wouldn’t work after each month as the starting value (the DD - Day) will start from 01 again. If you don’t get me, it doesn’t matter because we’re just making dates look nice in this post anyway.


### Human Readable

When I say ‘Human Readable’ I don’t mean that the particular date (for example) ‘2006-03-05 11:00:00? isn’t readable, but you wouldn’t necessarily read it out as ‘two-zero-zero-six zero-three zero-five one-one zero-zero zero-zero’ would you? A human readable version of this date would be something like Sunday, 05 March, 2006 @ 11:00:00. So how do we convert from one format to the next? Easy! Like this:

{% highlight php %}
<?php
$sNewDate = date("D, d F, Y @ H:i:s", strtotime('2006-03-05 11:00:00'));
?>
{% endhighlight %}

Now if we output the value of the new date variable ‘$sNewDate’:

{% highlight php %}
<?php
print $sNewDate;
?>
{% endhighlight %}

We get:

> Sun, 05 March, 2006 @ 11:00:00

We could of course use:

{% highlight php %}
<?php
date("D, d F, Y @ H:i:s");
?>
{% endhighlight %}

Which would output a nicely formatted date for the current date and time. You can also use other time strings for the second parameter, as long as you use strtotime() to format it into a nice UNIX timestamp for the date() function to process.


### The Magic

So how did I make PHP output the particular parts of the date with extras like commas (,) and the at sign (@)? Let’s look at PHP’s date() function, which takes 2 arguments:

* **string format** The format that date() should output
* **int timestamp (optional)** The timestamp for date to format the date. If this is left blank, it will default to the current date and time.

The string format takes preset PHP characters, and will allow other characters provided they’re not in the ‘preset’ list, and if they are they must be escaped. Taking our example into account, lets see what characters we used:

> "D, d F, Y @ H:i:s"

We used:

* D - A textual representation of a day, three letters
* d - Day of the month, 2 digits with leading zeros
* F - A full textual representation of a month, such as January or March
* Y - A full numeric representation of a year, 4 digits
* H - 24-hour format of an hour with leading zeros
* i - Minutes with leading zeros
* s - Seconds, with leading zeros

View [the full list of characters](http://uk.php.net/date) to learn more about PHP’s date() function. Now how did we get the commas and @ signs in there? Simple, you can just add these anywhere you like within the ‘format string’ as I did above. If you wanted to actually output text within the formatted string, you could do something like this:

{% highlight php %}
<?php
print date("\D\a\\t\e\: D, d F, Y @ H:i:s");
?>
{% endhighlight %}

or

{% highlight php %}
<?php
print 'Date: ' . date("D, d F, Y @ H:i:s");
?>
{% endhighlight %}

Notice how, in the first example, we ‘escape’ each _literal _character with a backslash (\), and how we had to ‘double-escape’ the ‘t’ as ‘**\t**‘ sends a **TAB **to the page, so if we want a ‘t’ we must use ‘**\\t**‘. Both of the above output:

> Date: Sun, 05 March, 2006 @ 11:00:00

I hope this has given you some insight on how easy it is to format dates using PHP. Any questions, just comment on this post!