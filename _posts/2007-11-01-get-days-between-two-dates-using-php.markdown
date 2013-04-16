---
author: admin
comments: true
date: 2007-11-01 22:55:24
layout: post
slug: get-days-between-two-dates-using-php
title: Get Days Between Two Dates Using PHP
wordpress_id: 4
categories:
- php
tags:
- code
- dates
- php
---

This is a simple snippet of code that will return an array of days between two dates.

{% highlight php %}
<?php
function GetDays($sStartDate, $sEndDate){
  // Firstly, format the provided dates.
  // This function works best with YYYY-MM-DD
  // but other date formats will work thanks
  // to strtotime().
  $sStartDate = gmdate("Y-m-d", strtotime($sStartDate));
  $sEndDate = gmdate("Y-m-d", strtotime($sEndDate));

  // Start the variable off with the start date
  $aDays[] = $sStartDate;

  // Set a 'temp' variable, sCurrentDate, with
  // the start date - before beginning the loop
  $sCurrentDate = $sStartDate;

  // While the current date is less than the end date
  while($sCurrentDate < $sEndDate){
    // Add a day to the current date
    $sCurrentDate = gmdate("Y-m-d", strtotime("+1 day", strtotime($sCurrentDate)));

    // Add this new day to the aDays array
    $aDays[] = $sCurrentDate;
  }

  // Once the loop has finished, return the
  // array of days.
  return $aDays;
}
?>
{% endhighlight %}


###Usage:

{% highlight php %}
<?php
$aDays = GetDays('5th Feb 2007', '10th Feb 2007');
?>
{% endhighlight %}

You can use most date formats, such as:

{% highlight php %}
<?php
GetDays('2007-01-01', '2007-01-31');
?>
{% endhighlight %}

or

{% highlight php %}
<?php
GetDays('19-02-2007', '25-02-2007');
?>
{% endhighlight %}

Whatever strtotime can handle, you can pass to this function.

Hope it helps!
