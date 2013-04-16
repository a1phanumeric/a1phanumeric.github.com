---
author: admin
comments: true
date: 2010-07-11 17:18:18
layout: post
slug: how-to-make-url-safe-strings-for-mod_rewrite-using-php
title: How to make URL Safe strings for mod_rewrite using PHP
wordpress_id: 31
categories:
- php
tags:
- mod_rewrite
- htaccess
- php
---

It's relatively easy to make URL safe strings for use by mod_rewrite. Let's use the example that you have a form that adds a new blog post to your site. When the user submits this form, you want to generate a URL safe string (based on the title of the blog post) for mod_rewrite to use. This little snippet will show you how this can be achieved in one line of code in PHP.

The following function shows just how easy it is to generate a URL safe string for mod_rewrite.

{% highlight php %}
<?php
function MakeURLSafeString($string){
	return trim(preg_replace('/[-]{2,}/', '-', preg_replace('/[^a-z0-9]+/', '-', strtolower($_POST['TheString']))), '-');
}
?>
{% endhighlight %}

That's all there is to it! To break it down a little, you could have written it as per the following:

{% highlight php %}
<?php
function MakeURLSafeString($string){
	$string = strtolower($string); // Makes everything lowercase (just looks tidier).
	$string = preg_replace('/[^a-z0-9]+/', '-', $string); // Replaces all non-alphanumeric characters with a hyphen.
	$string = preg_replace('/[-]{2,}/', '-', $string); // Replaces one or more occurrences of a hyphen, with a single one.
	$string = trim($string, '-'); // This ensures that our string doesn't start or end with a hyphen.
	return $string;
}
?>
{% endhighlight %}



