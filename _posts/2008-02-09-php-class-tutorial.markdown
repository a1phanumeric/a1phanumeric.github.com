---
author: admin
comments: true
date: 2008-02-09 00:39:09
layout: post
slug: php-class-tutorial
title: PHP Class Tutorial
wordpress_id: 13
categories:
- php
tags:
- beginner PHP classes tutorial
- classes
- oop
- php
- tutorial
---

Easy to follow PHP class tutorial (which is Object Orientated Programming - OOP). I’m not sure how to start this one... It can be quite difficult to understand PHP classes at first, but hopefully I’ll make everything seem easy! Let's just get stuck in shall we...

### PHP Class Tutorial Chapters

1. **Part 1 - Jumping In With Two Feet**
2. [Part 2 - What does $this-> mean in a PHP class file?]({{ site.url }}/php/php-class-tutorial-part-2-what-is-this/)  
3. [Part 3 - What Are Class Constructors?]({{ site.url }}/php/php-class-tutorial-part-3-what-are-class-constructors/)

# Brief overview…

Ok so you’re actually reading this php class tutorial overview? You must be serious about learning how to create and use PHP classes…

Firstly, when you hear someone talking about PHP with Object Orientated Programming, or OOP as it's otherwise known, they are in-fact referring to PHP classes. PHP classes can be used to group together a set of ‘like’ functions used within a bigger application. Their main advantage is the fact that you can edit the particular class function, or functions and make a site-wide change. Classes also help give you a more structured approach to programming, and those that like to hack with some GPL/MIT released web applications (with PHP classes) will have a much better understanding of the workings of them.

This may not be the best example of explaining why to use classes in PHP, but it’s an example of how to use them.

To begin our PHP class tutorial, let’s start by creating a new file called time.php. Within this file, let’s add some simple code (this isn't a PHP class yet!):

{% highlight php %}
<?php
$sTime = gmdate("d-m-Y H:i:s");
print 'The time is: ' . $sTime;
?>
{% endhighlight %}

This will simply assign the current date and time to the variable **$sTime** and then print the string 'The time is' with the variable value at the end (e.g. The time is: 09-02-2007 21:42:28).

How would we do this, using a class? Well there's many ways, however I would recommend using the class file to generate the time, then use the acutal 'action page' (time.php) to output the time. Let's create our class file!

# Get in class!

So, now we're going to make the above into a function, which will be placed inside our new PHP class file, for future use. Create a new file (keep it in the same directory for this tutorial). Let’s call it **class.Time.php**. Add the following code:


{% highlight php %}
<?php
class Time {
  function GenerateCurrentTime(){
    $sTime = gmdate("d-m-Y H:i:s");
    return $sTime;
  }
}
?>
{% endhighlight %}

Lets do this line by line... The first line, **class Time {**,declares the class as open (exactly the same as a function in PHP, but without the brackets in this case). This tells PHP that we have a new class, and we're calling it 'Time'.

The next line declares a new function. The difference here is that it exists ONLY within the scope of the class (e.g. it's built WITHIN the PHP class). We then generate the time as we did before, assigning it to the variable $sTime and then return the value of this variable. The function then closes, followed by the class closure (the squiggly brackets '}', or "close-stache"). Note that our class needs to also be wrapped in PHP tags (`<?php ... ?>`).

Now open the original file, time.php, and change the code to match the following:

{% highlight php %}
<?php
include ('class.Time.php');
$oTime = new Time;
$sTime = $oTime->GenerateCurrentTime();
print 'The time is: ' . $sTime;
?>
{% endhighlight %}

Now, the first line here includes the time PHP class file (**include ('class.Time.php');**). We must include all the PHP class files we wish to take advantage of, otherwise how the hell would PHP know about these files?

The next line, $oTime = new Time, creates the class object and stores it in the variable $oTime. Notice, to store the class in an object variable, we use _VARIABLE = NEW CLASSNAME_. _VARIABLE_ can be anything, then there must be an equals sign '='. _NEW _must use 'new' or '&new;', and the _CLASSNAME _must match the name of the class. In this case, the name of the class is Time (case sensitive - as PHP is throughout). The PHP class name is 'Time' because we created the class using **class Time {**.

If we had used **class HelloWorld {**, as you can guess, the PHP class name would be 'HelloWorld'.

Anyway… now we've created our class, we have also included it within the page we want to make use of it. Not only that, we have ALSO initalised our class by defining it in an object variable - $oTime. Now, it's not completely covered within in the scope of this PHP class tutorial, but you can kind-of think of $oTime being a variable which stores functions that we can do many things with.

So, the next line:

{% highlight php %}
<?php
$sTime = $oTime->GenerateCurrentTime();
?>
{% endhighlight %}

This simply assings the variable `$sTime` with the result of the function GenerateCurrentTime() within the Time class. How does it do this? Simple… We want to use the function GenerateCurrentTime() within the class $oTime so we simply us:

{% highlight php %}
<?php
$oTime->GenerateCurrentTime();
?>
{% endhighlight %}

This tells PHP exactly what we want to do. The '->' explains to PHP that the prefix (in this case $oTime, which we know holds the class object) is the parent of the latter (again, in this case the latter is GenerateCurrentTime()). So it basically means, run GenerateCurrentTime() within the $oTime class. Thus assigning whatever is returned by the function GenerateCurrentTime() to the variable $sTime.

The last line does what we did from scratch… print out the results with the prefixed string 'The time is'.

In the next PHP class tutorial installment, we will discuss what $this-> means, and how it can be immensely beneficial to you to use PHP classes!

### PHP Class Tutorial Chapters

1. **Part 1 - Jumping In With Two Feet**
2. [Part 2 - What does $this-> mean in a PHP class file?]({{ site.url }}/php/php-class-tutorial-part-2-what-is-this/)  
3. [Part 3 - What Are Class Constructors?]({{ site.url }}/php/php-class-tutorial-part-3-what-are-class-constructors/)
