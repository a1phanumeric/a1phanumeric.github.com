---
author: admin
comments: true
date: 2008-03-06 00:23:39
layout: post
slug: top-10-htaccess-tips-and-tricks
title: Top 10 .htaccess Tips and Tricks
wordpress_id: 18
categories:
- apache
- htaccess
tags:
- htaccess
- apache
- tips
---

## Custom Error Documents

Creating custom documents gives your site a more professional look, as not only are you providing a 'net' to catch unsuspecting visitors when they follow a bad link and such like, but they also allow you to customise the style of the page so you can maintain your basic site design by adding HTML.

{% highlight apache %}
# custom error documents
ErrorDocument 401 /error/401.php
ErrorDocument 403 /error/403.php
ErrorDocument 404 /error/404.php
ErrorDocument 500 /error/500.php
{% endhighlight %}

## Control Access

Being able to control access to certain areas of your server can be very useful. The following example demonstrates how to only allow access from those connecting from a 192.168.0 LAN IP pool. This could be easily modified to only allow access from a single remote IP address or addresses.

{% highlight apache %}
# no nasty crackers in here!
order deny,allow
deny from all
allow from 192.168.0.0/24
# this would do the same thing..
#allow from 192.168.0
{% endhighlight %}

## Hide and Deny Files

Hiding and denying access to files is crucial to servers that have sensitive data held within files that may be accessible via the website(s) on it. The following example demonstrates how to prevent acces to any files ending with .log - and is case insensitive (i.e. .LoG / .lOG / .loG).

{% highlight apache %}
Order allow,deny
Deny from all
Satisfy All
{% endhighlight %}

## Basic Rewriting

I have written a [mod_rewrite tutorial]({{ site.url }}/apache/htaccess/beginners-mod_rewrite-tutorial/), but this is worth a mention as a top 10 tip for .htaccess files as it's becoming more and more commonly used these days - primarily for SEO purposes.

This example will redirect a request for **{{ site.url }}/page_one.htm** to **{{ site.url }}/page_one.php**. The _r=301_ tells apache to send a proper HTTP _Permanently Moved_ redirection (301), which will update the address bar in the browser window to show 'page_one.php'. Without this, you'd still see 'page_one.htm' even though you're seeing a PHP page. This helps SEO, as spiders and search engines will update their listings to reflect the PHP versions.

{% highlight apache %}
Options +FollowSymlinks
RewriteEngine on
RewriteRule ^(.+)\.htm$ {{ site.url }}/$1.php [r=301,nc]
{% endhighlight %}

## Shorter URLs

Shorter URLs are beneficial, as visitors that persist in typing full URLs won't have to type as much, and they're more memorable. Do they benefit SEO, even though the full URL contains the same keywords? I don't know, maybe someone can tell me.

This example will rewrite a page requested as **{{ site.url }}/files/code/apache.zip** to **{{ site.url }}/download.php?type=code&file=apache**.

{% highlight apache %}
Options +FollowSymlinks
RewriteEngine on
RewriteRule ^files/(.+)/(.+).zip download.php?type=$1&file;=$2 [nc]
{% endhighlight %}

## Prevent Hotlinking

Preventing hotlinking can reduce bandwidth, by disallowing other websites from displaying images hosted on your server. The following rule basically says that if the referer is NOT edrackham.com, run the following rule. The rule (on the next line) says that if the request is for a .gif, .jpg or.png then redirect the visitor to {{ site.url }}/img/hotlink_f_o.png. I'll leave you to work out what the 'f_o' stands for.

{% highlight apache %}
Options +FollowSymlinks
# no hot-linking
RewriteEngine On
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http://(www\.)?edrackham\.com/ [nc]
RewriteRule .*\.(gif|jpg|png)$ {{ site.url }}/img/hotlink_f_o.png [nc]
{% endhighlight %}

## Hiding Page Extension

Similar to the mod_rewrite code above, this will redirect a request for **product-_3_.html** to **products.php?id=_3_**. As we're not using _r=301_, the requested page will remain in the browser's address bar.

{% highlight apache %}
Options +FollowSymlinks
RewriteEngine on
RewriteRule ^product-([0-9]+)\.html$ products.php?id=$1
{% endhighlight %}

## Ban Selected User Agents

In my opinion, it'd be ace to block all requests from a Microsoft user agent, but alas, that wouldn't be too cool as some people are still hell-bent on using a non-standards compliant browser. Having said that, Microsoft is making their new [IE8 release standards compliant]({{ site.url }}/ie8/standards-compliant-internet-explorer-ie8/) by default.

The following provides some examples for blocking requests to your server from certain user agents.

{% highlight apache %}
#####################################
# Deny Useragents
#####################################

RewriteCond %{HTTP_USER_AGENT} ^FrontPage [NC,OR]
RewriteCond %{HTTP_USER_AGENT} ^Java.* [NC,OR]
RewriteCond %{HTTP_USER_AGENT} ^Microsoft.URL [NC,OR]
RewriteCond %{HTTP_USER_AGENT} ^MSFrontPage [NC,OR]
RewriteCond %{HTTP_USER_AGENT} ^Offline.Explorer [NC,OR]
RewriteCond %{HTTP_USER_AGENT} ^[Ww]eb[Bb]andit [NC,OR]
RewriteCond %{HTTP_USER_AGENT} ^Zeus [NC]
RewriteRule ^.*$ - [F]
{% endhighlight %}

## Making Other Filetypes Executable

Ever wanted to make your site look like it runs off a new language such as .w00t files? Well you can with .htaccess! Adding this neat one-liner, you can request .w00t files, which will be served and interpreted as .php type files.

{% highlight apache %}
AddType application/x-httpd-php .w00t
{% endhighlight %}

## Force Media Downloads

Sometimes, when clicking on media files, the browser wants to play or stream it directly from itself. Using the following rules, media files (.avi/.mpg/.wmv/.mp3 in this example) will provide a download dialog box instead.

{% highlight apache %}
# instruct browser to download multimedia files
AddType application/octet-stream .avi
AddType application/octet-stream .mpg
AddType application/octet-stream .wmv
AddType application/octet-stream .mp3
{% endhighlight %}

## Require SSL

Sometimes you will require an SSL connection. This following snippet will do just that!

{% highlight apache %}
# require SSL
SSLOptions +StrictRequire
SSLRequireSSL
SSLRequire %{HTTP_HOST} eq "domain.tld"
ErrorDocument 403 https://domain.tld

# require SSL without mod_ssl
RewriteCond %{HTTPS} !=on [NC]
RewriteRule ^.*$ https://%{SERVER_NAME}%{REQUEST_URI} [R,L]
{% endhighlight %}

Sources:
- http://corz.org/serv/tricks/htaccess.php
- http://roshanbh.com.np/2008/02/hide-php-url-rewriting-htaccess.html
- http://expressproducts.net/htaccess.htm
- http://perishablepress.com/press/2006/01/10/stupid-htaccess-tricks/#usa4
- http://phpsecurity.wordpress.com/2007/12/22/htaccess-tips-and-tricks/
