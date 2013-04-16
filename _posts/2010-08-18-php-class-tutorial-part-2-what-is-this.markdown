---
author: admin
comments: true
date: 2010-08-18 18:44:47
layout: post
slug: php-class-tutorial-part-2-what-is-this
title: PHP Class Tutorial - Part 2 - What is $this->
wordpress_id: 28
categories:
- php
tags:
- class
- oop
- php
- tutorial
---

Please make sure you've followed my [first PHP Class Tutorial]({{ site.url }}/php/php-class-tutorial/) before starting this one, as it follows on using the [previous example]({{ site.url }}/php/php-class-tutorial/). This tutorial will explain what $this-> is all about, and how to further your PHP class knowledge!

### PHP Class Tutorial Chapters

1. [Part 1 - Jumping In With Two Feet]({{ site.url }}/php/php-class-tutorial/)  
2. **Part 2 - What does $this-> mean in a PHP class file?**  
3. [Part 3 - What Are Class Constructors?]({{ site.url }}/php/php-class-tutorial-part-3-what-are-class-constructors/)

# I'm ready to go, set me up!

Our first example used the following - very simple - class file:

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


Now, let's add in the ability for our class file to look **X** number of days into the future shall we? Add in a new function to the class file that looks like:

{% highlight php%}
<?php
  function ShowFutureDate($iAddDays=0){
    $sTime = gmdate("d-m-Y H:i:s", strtotime("+" . $iAddDays . " days"));
    return $sTime;
  }
?>
{% endhighlight %}

Now, if we were to run

{% highlight php %}
<?php
ShowFutureDate(5);
print 'The time in 5 days is: ' . $sTime;
?>
{% endhighlight %}

We'd see the date in 5 days time. But for the purposes of this tutorial, let's change things slightly so that we can use `$this->` and start to understand it!

Firstly, let's declare a variable that we can use within any of the class functions. We accomplish this in the following way:

{% highlight php %}
<?php
class Time {

  var $sTime;

  function GenerateCurrentTime(){
    $sTime = gmdate("d-m-Y H:i:s");
    return $sTime;
  }
  
  function ShowFutureDate($iAddDays=0){
    $sTime = gmdate("d-m-Y H:i:s", strtotime("+" . $iAddDays . " days"));
    return $sTime;
  }
}
?>
{% endhighlight %}

Notice how we added `var $sTime;` at the beginning of the class file? Do the same in your code.

Let's start using this as a variable, available only to the scope of the class file. It's good practice to declare your class variables (only accessible within the scope of the class file / functions) in the header. Why are you calling that bit of space between the opening curly brace and the first function the header I hear you all ask? It's just what I call it, as do many other developers. It's a nice bit of white-space where you can declare your class variables within the PHP class. 

# Ok done that, so what is $this->?

Right, we're going to remove the references to `$sTime` in both of our functions we now have, and we're going to replace it with `$this->sTime`. Our entire class file should now look like the following:

{% highlight php %}
<?php
class Time {

  var $sTime;

  function GenerateCurrentTime(){
    $this->sTime = gmdate("d-m-Y H:i:s");
    return $this->sTime;
  }
  
  function ShowFutureDate($iAddDays=0){
    $this->sTime = gmdate("d-m-Y H:i:s", strtotime("+" . $iAddDays . " days"));
    return $this->sTime;
  }
}
?>
{% endhighlight %}

If you run the code, and execute the functions on a page where you have this class file included (not within this class file!) - you'll see that we're still returning the same shizzle as before. That's fine. Good infact - it means you're following along well! Let's say that you now run the function:

{% highlight php %}
<?php
$oTime->ShowFutureDate(5);
?>
{% endhighlight %}

We know that the class, `$oTime` has now set it's class variable - `$sTime` - to the current date + 5 days. We can now use that variable ANYWHERE in our page without having to run that function again, and again. THAT my friends is the beauty of class variables. As long as you have executed that function, we can now use:


{% highlight php %}
<?php
echo $oTime->sTime;
?>
{% endhighlight %}

Anywhere, and as many times in the page as we like. It makes for much faster code! If you think of `this->` outside of the class as the actual class name - `$oTime`, then you will have no problem using `$this->` within your class files, to write much much more efficient code.

# Final PHP Class File Code

I's worth noting in this PHP class file tutorial, that we should really remove the **return** references in our code, as we don't really need them anymore. Our final code should now look like this:

{% highlight php %}
<?php
class Time {

  var $sTime;

  function GenerateCurrentTime(){
    $this->sTime = gmdate("d-m-Y H:i:s");
  }
  
  function ShowFutureDate($iAddDays=0){
    $this->sTime = gmdate("d-m-Y H:i:s", strtotime("+" . $iAddDays . " days"));
  }
}
?>
{% endhighlight %}

The next tutorial will talk about **Class Constructors**!

### PHP Class Tutorial Chapters

1. [Part 1 - Jumping In With Two Feet]({{ site.url }}/php/php-class-tutorial/)  
2. **Part 2 - What does $this-> mean in a PHP class file?**  
3. [Part 3 - What Are Class Constructors?]({{ site.url }}/php/php-class-tutorial-part-3-what-are-class-constructors/)
