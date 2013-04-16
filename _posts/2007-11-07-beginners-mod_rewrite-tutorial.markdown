---
author: admin
comments: true
date: 2007-11-07 14:26:38
layout: post
slug: beginners-mod_rewrite-tutorial
title: Beginners Mod_Rewrite Tutorial
wordpress_id: 7
categories:
- apache
- htaccess
---

Have you seen those websites such as [Reddit](http://reddit.com) that use fancy URLs such as http://domain.com/**page_content**? Yeah, sure this could be *nearly achieved* by simply renaming your pages to something like **page_content.htm** - but that would result in you having to change every page on your site to a suitably written filename, as well as relying on static pages. I'm going to attempt to demonstrate just how easy it really is to utilise mod_rewrite to have fancy - even "web 2.0" - URLs within your site.

Mod_rewrite is useful for pages with dynamic content, such as those which rely on either POST or GET data to generate the content. A URL that looks something like **http://domain.com/product.php?product_id=19** would be ideal to control using mod_rewrite. The main reasons being that:

1. The URL is referencing dynamic content (as the GET suggests in the URL).
2. The page, product.php, will remain the same - yet load different data based on the value of the GET variable 'product_id'.

So let's get started shall we? If you know mod_rewrite works, start masking pages with mod_rewrite now!.

### Testing Mod_Rewrite Works

In order to use mod_rewrite, you will need the following:

* The module 'mod_rewrite' enabled.
* Either have direct access to your httpd.conf file _which won't be entirely discussed in this tutorial_, or have the ability to use .htaccess files within your site.
* Have the AllowOverride directive within the httpd.conf file set to allow rules for you to use mod_rewrite commands.

Don't panic - most Apache web servers have the above enabled and ready to go! Let's do a simple test to make sure everything is cool to go. Create a .htaccess file in your site's root directory.

Insert the following text into your new .htaccess file:

{% highlight apache %}
Options +FollowSymLinks
Options +Indexes
RewriteEngine On
{% endhighlight %}

> The .htaccess file will affect all files within the same directory as itself, as well as sub-directories and their respective files. The directory can change, but for the purposes of this tutorial, we're gonna stick this bad boy in your root directory.

Upload your .htaccess file to the root directory of your site, and load the index page (well, any valid page for that matter!). One of two things will happen (at least, I think I'm right here...). You'll find that either:

* Your requested page loads just fine. **Good**.
* You get an Internal Server Error (500). **Bad**.

If you get the Internal Server Error (error 500), I'm afraid that it looks like either mod_rewrite isn't installed, or your httpd.conf file doesn't allow the required AllowOverride directives. If this is the case, and you _do _have access to edit your httpd.conf file, do so and set AllowOverride to ALL for your site root directory:

{% highlight apache %}
<Directory "/var/www/sites/www.domain.com">
AllowOverride All
{% endhighlight %}

> You can also add all of the mod_rewrite rules within the httpd.conf file instead of the .htaccess file if you so wish. My preference is to use the .htaccess file as it's more portable, and generally easier to edit.

After you've edited the httpd.conf file / contacted your server guys or whatever - you should be good to go. If not, I'm all out of ideas. Sorry. Try [Google](http://www.google.com/search?q=mod_rewrite+500+error) for help :].

Assuming all's working well, let's get to the cool bits!

### Masking Pages with Mod_Rewrite

This is kind of pointless, but is a simple demonstration for this tutorial. If you don't care much for pointless, simple-to-understand demonstrations, go straight to the useful rewrite rules.

First create two basic HTML pages. Name them '[page_one.htm](http://www.edrackham.com/tutorials/beginners_mod_rewrite_tutorial/page_one.htm)' and '[page_two.htm](http://www.edrackham.com/tutorials/beginners_mod_rewrite_tutorial/page_two.htm)'. Add a little dummy content to both of them, but make sure they have different content (this is so as we can differentiate the pages). Open your .htaccess file, and if you don't have the main opening lines:

{% highlight apache %}
Options +FollowSymLinks
Options +Indexes
RewriteEngine On
{% endhighlight %}

Add them now. On the next free line, add the following:

{% highlight apache %}
RewriteRule ^page_one.htm$ page_two.htm [L]
{% endhighlight %}

Now, load page one. What do you see? Page two hopefully. How? Well, the RewriteRule breaks down like this:

{% highlight apache %}
RewriteRule [FIND THIS] [REPLACE WITH THIS] [FLAGS]
{% endhighlight %}

RewriteRules have 4 main parts to them. The rewrite action:

{% highlight apache %}
RewriteRule
{% endhighlight %}

Then the FIND THIS part:

{% highlight bash %}
^page_one.htm$
{% endhighlight %}

This is a regular expression. Everything found in between the circumflex (^) and dollar sign ($) will be searched for and saved as a variable (more on this later). If the FIND THIS value is found, it'll be replaced with the third part of the RewriteRule - the REPLACE WITH THIS:

{% highlight bash %}
page_two.htm
{% endhighlight %}

The fourth and final part of the RewriteRule is the flags. You can put flags in between square brackets ([ ]) to control how mod_rewrite should deal with the rule itself.

{% highlight bash %}
[L]
{% endhighlight %}

I've simply used one flag, 'L'. This means that this is the last rule. As we only have one rule at the moment, using this flag seems appropriate, as it's the only, and last, rule. You can use multiple flags, by comma delimiting them. There is a list of the flags at the end of this tutorial.

### Rewriting Dynamic URLs

Let's take the first URL mentioned in this tutorial as the example for this part of the tutorial: **http://domain.com/product.php?product_id=19**

Create a new PHP page called product.php. Add the following code between the  tags:

{% highlight html %}
You are viewing product <?php echo $_REQUEST['product_id']; ?>
{% endhighlight %}

Save, and upload your file. You can [view my example of product.php](http://www.edrackham.com/tutorials/beginners_mod_rewrite_tutorial/product.php?product_id=19) to see how the page changes with the dynamic content from the GET request.

Let's say you would prefer to use a cleaner URL, such as: **http://domain.com/product/19/**

This could be achieved by adding the following line to your .htaccess file:

{% highlight apache %}
RewriteRule ^product/([0-9]+)/$ product.php?product_id=$1 [NC,L]
{% endhighlight %}

You can [test this in action](http://www.edrackham.com/tutorials/beginners_mod_rewrite_tutorial/product/25/) if you wish. If we break down the RewritRule into the four parts again, you should be able to see how it's working:

{% highlight apache %}
RewriteRule
{% endhighlight %}

Here, we're simply stating that we're adding a new rule for mod_rewrite to use.

{% highlight bash %}
^product/([0-9]+)/$
{% endhighlight %}

As before, this is the FIND THIS value. We are looking for a string within the URL that contains product/[NUMERICAL VALUE]/ - that is, the word 'product', then a forward-slash (/), then a numerical value only, then a final forward slash. The magic here lies between the brackets ( ):

{% highlight bash %}
([0-9]+)
{% endhighlight %}

The brackets ( ) are telling mod_rewrite to **store whatever it finds within the brackets in a variable**. The [0-9]+ is a simple regular expression allowing only numbers 0 to 9, and the + sign means 'any number of'. So we can have 99 or 123 for example, **not just one number**, such as a 9 or a 1.

We finally add a trailing slash (/) at the end to finish what we're looking for (as our desired clean URL is  **product/[PRODUCT ID]/**).

{% highlight bash %}
product.php?product_id=$1
{% endhighlight %}

The third part of our RewriteRule is the REPLACE WITH THIS part. Here you can see the actual page name, with the query string - but this time there's something new. Where we used to have the product ID, we now have a variable $1. $1 will contain the value of the result of the regular expression found within the brackets in the FIND THIS part. Can you see it all falling into place now?

{% highlight bash %}
[NC,L]
{% endhighlight %}

Finally come the flags. I've used two flags here, NC meaning that the rule is case insensitive (it just protects us incase anyone uses  **http://domain.com/Product/19/ **for example), and L meaning it's our last rule.

Feel free to [check this out with a working example](http://www.edrackham.com/tutorials/beginners_mod_rewrite_tutorial/product/19/). Change the number from 19 to a different number and see what happens.

### Mod_Rewrite Flags

* C - Chain
* E - Environmental Variable
* F - Forbidden
* G - 410 Gone
* L - Last
* N - Next (Round)
* NC - No Case
* NE - No Escape
* NS - No SubRequest
* P - Proxy
* PT - Pass Through
* QSA - Query String Append
* R - Redirect
* S - Skip
* T - Type

### Closure

I could go on and on with more examples of mod_rewrite, but that would exceed the scope of this tutorial - seeing as this is only a beginner's tutorial. Through this tutorial, I hope that you've learned enough of the basics in order to get you started. You should now know:

 * How to get mod_rewrite working.
 * How to mask pages.
 * How to create cleaner URLs for dynamic pages.

### Useful Reading & References

* [Apache's Mod_Rewrite Documentation](http://httpd.apache.org/docs/1.3/mod/mod_rewrite.html)
* [Mod_Rewrite Cheat Sheet](http://www.ilovejackdaniels.com/mod_rewrite_cheat_sheet.png)
* [Regular Expression Tutorials](http://www.regular-expressions.info/)


