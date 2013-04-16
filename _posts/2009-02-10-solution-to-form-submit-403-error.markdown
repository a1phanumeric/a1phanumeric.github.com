---
author: admin
comments: true
date: 2009-02-10 14:51:25
layout: post
slug: solution-to-form-submit-403-error
title: Solution to Form Submit 403 Error
wordpress_id: 27
categories:
- javascript
tags:
- mod_security
- bugfix
---

- Have you ever had a problem where you get a HTTP 403 error from submitting a form?
- Does this form have a field that submits a URL?

If **yes** to the above two questions, I think I know the problem you're having, and have a solution. It's to do with [mod_security](http://www.modsecurity.org/) (an Apache module) and the 'http://' part of the URL.

The obvious answer is to tell you to go and modify the module yourself, but many of you don't have access to those kind of modifications due to hosting company restrictions. My simple solution uses JavaScript to remove the 'http://' part from the URL.

Let's start by saying you have a form that looks something like the following:

{% highlight html %}
<form id="SendDataForm" name="SendDataForm" method="post" action="page.php">
	<label for="FullName">Full Name:</label>
	<input type="text" name="FullName" id="FullName">
	<br>
	<label for="EmailAddress">Email Address:</label>
	<input type="text" name="EmailAddress" id="EmailAddress">
	<br>
	<label for="WebsiteAddress">Website Address:</label>
	<input type="text" name="WebsiteAddress" id="WebsiteAddress">
	<br>
	<label for="SendForm"> </label>
	<input type="submit" name="SendForm" id="SendForm" value="Submit">
</form>
{% endhighlight %}


Add the following between javascript in the head, foot, or within a JavaScript include:

{% highlight javascript %}
function trim(str, chars) {
	return ltrim(rtrim(str, chars), chars);
}

function ltrim(str, chars) {
	chars = chars || "\\s";
	return str.replace(new RegExp("^[" + chars + "]+", "g"), "");
}

function rtrim(str, chars) {
	chars = chars || "\\s";
	return str.replace(new RegExp("[" + chars + "]+$", "g"), "");
}

function MakeLinkSafe(){
	var e = document.getElementById('WebsiteAddress')
	str = trim(e.value);
	if(str.substr(0, 7) == 'http://'){
		e.value = str.substr(7);
	}
	return true;
}
{% endhighlight %}

Then, on your form submit button, add the following:

{% highlight javascript %}
onclick="MakeLinkSafe()"
{% endhighlight %}

So that your submit button now looks like:

{% highlight html %}
<input type="submit" name="SendForm" id="SendForm" value="Submit" onclick="MakeLinkSafe()">
{% endhighlight %}

Now, when you click the submit button, it'll invoke the JavaScript, which will remove the http:// part (if it exists). You can then modify your 'action page' code to catch this element and re-add the http:// part if needs be.

Just remember to update the

{% highlight javascript %}
document.getElementById('WebsiteAddress')
{% endhighlight %}

to reflect the ID of the element that collects the URL on your form.
