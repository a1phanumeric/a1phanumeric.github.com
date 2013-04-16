---
author: admin
comments: true
date: 2007-11-05 16:44:42
layout: post
slug: basic-php-tutorial
title: Basic PHP Tutorial
wordpress_id: 5
categories:
- php
tags:
- basics
- beginner
- php
---

Ok, so how do you load a .php file into your web browser, and what does it do when you type (for example) http://www.edrackham.com/index.php then hit enter?

1. The web browser requests the document with the .php extension from the edrackham.com server.
2. The webserver kicks in and says "Hey! Someone wants a PHP file. Something else needs to deal with it because I only serve HTML", and then sends the request onto a PHP parser software located on the server.
3. The PHP parser finds the PHP file, then scans it for PHP code from top to bottom, left to right (how we read … that is if you're English).
4. When the PHP parser finds PHP code, it executes it and places the output (if any) into the place in the file formerly occupied by the code.
5. This new output is sent back to the webserver.
6. The webserver then sends it along to the web browser.
7. The web browser displays the output.

PHP can be used within a file with the extension '.php'. There are other valid extensions such as '.php3' and '.phps' but I'm using '.php' as it’s widely and more commonly used. ALL PHP code MUST be entered within PHP "start and end" tags. N.B. The '...' represents php code. There are three main PHP "start and end" tags, shown below:

{% highlight html+php %}
<?php ... ?>
<? ... ?>
<script language="php"> ... </script>
{% endhighlight %}

Option 1 (`<?php ... ?>`) is the most common, and will be used throughout the remainder of this tutorial. A simple PHP script may look like either of the following:

{% highlight html+php %}
<?php
echo "This is my first PHP script. w00t";
?>
{% endhighlight %}

Or

{% highlight php %}
<?php
echo "<p> This is my first PHP script. w00t</p>"
?>
{% endhighlight %}

`echo` is a built-in function within PHP (as are MANY MANY others). echo simply prints whatever follows into the document... in this case,  `<p>This is my first PHP script. w00t</p>`. This would  obviously display the text 'This is my first PHP script. w00t' in a web  browser, using the `<p></p>` HTML tags.

Nearly every line in PHP is ended with a `;` (semi-colon). Only 'instructors' (a piece of code telling PHP what to do) needs a terminator `;`. This is much the same for languages such as C, and JavaScript. A piece of code like the following shows an example of how not EVERY line needs a terminator:

{% highlight php %}
<?php
for($i=0;$i<10;$i++){  
	echo "I am line number ".$i."<br />";  
}
?>
{% endhighlight %}

As you can see only the 'echo' function line uses a terminator, not the 'for' loop, nor the closing curly bracket. Out of curiosity, the above function would produce:

{% highlight c %}
I am line number 0
I am line number 1
I am line number 2
I am line number 3
I am line number 4
I am line number 5
I am line number 6
I am line number 7
I am line number 8
I am line number 9
{% endhighlight %}

### Commenting your code

You can use 3 types of comments:

{% highlight php %}
<?php
// Simple PHP Comment.  
/* This is a 'C' style comment which can be as long as you like, and on multiple lines. */  
# Used to Shells? Use this kind of comment for one-liners. 
?>
{% endhighlight %}


### Variables
A variable in PHP begins with the ‘$’ sign, and can be named ANYTHING you like as long as you stick to these golden rules:
1. Variables begin with a $ dollar sign
2. A name follows the $ dollar sign
3. The variable cannot begin with a numeric character.
4. The variable CAN contain numbers and even the underscore character.
5. Variable names are CaSe SeNsItIvE so $W00T and $w00t are different variables.

To declare a variable, think of a decent name:

{% highlight php %}
<?php
w00t
?>
{% endhighlight %}

Bang a **$** dollar sign infront of it:

{% highlight php %}
<?php
$w00t
?>
{% endhighlight %}

Use the equal sign (=) after it, and assign a literal value:

{% highlight php %}
<?php
$w00t="edrackham"
?>
{% endhighlight %}

Asigning a variable is an instruction, and as such, should have a terminator at the end of it:

{% highlight php %}
<?php
$w00t="edrackham"T-1000
?>
{% endhighlight %}

oops... I mean:

{% highlight php %}
<?php
$w00t="edrackham";
?>
{% endhighlight %}

There. **w00t** is now assigned the value "edrackham". One last thing to consider is that if you had a string, which included 'special characters' like the (") speechmark, you have to 'escape' the character. Observe:

{% highlight php %}
<?php
echo "My mate said "Hey!" the other day";
?>
{% endhighlight %}

Would result in an error because the PHP parser will begin to read along the line, it'd print "My mate said" then it meets another speechmark, this closes the echo session, and then mis-interperts the next word "Hey" as an invalid command or terminator. So how do we overcome this? We can "escape" special chars using the \ (backslash).

{% highlight php %}
<?php
echo "My mate said \"Hey!\" the other day";
?>
{% endhighlight %}

See... simple, we escaped the internal speechmarks using '\'. This function would now work no problemo. Search google for PHP special chars that need escaping.
