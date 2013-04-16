---
author: admin
comments: true
date: 2010-08-19 20:55:55
layout: post
slug: beginners-guide-to-writing-a-jquery-plugin
title: Beginner's Guide to Writing a jQuery Plugin
wordpress_id: 33
categories:
- jquery
tags:
- javascript
- jquery
- jquery-plugin
- tutorial
---

Lots of tutorials coming up in the next few days - seeing as you guys are actually enjoying them! This is a really simple tutorial on how to write a jQuery plugin. It's also a usable "Default Text" plugin, which sets some preset text in a form input, which clears when you focus the fiels.

It's not as comprehensive as some other articles out there, as I wanted to keep this nice and simple. If you're like me, you just want the simple tutorial - as you'll enjoy building on it in the future!

## Gimmie the code. Now.

Let's do what I usually do - start with the entire code snippet, so if you like - just take it and run! If you want a break down, keep reading on after the code.

- [Download the Plugin Here](http://github.com/a1phanumeric/jQuery-Default-Text-Plugin/archives/master)
- [Documentation / Usage Here](http://github.com/a1phanumeric/jQuery-Default-Text-Plugin/)
- [Demo the jQuery Default Text Plugin](http://edrackham.com/jquery/default_text/)

{% highlight javascript %}
$.fn.DefaultText = function(options) {

    var defaults = {
        inactiveClass:      '',
        inactiveFontStyle:  'italic',
        inactiveColour:     'rgb(204,204,204)',
        activeFontStyle:    'normal',
        activeColour:       'rgb(0,0,0)'
    };

    var opts = $.extend(defaults, options);
    
    $(this).each(function(){
        
        if(opts.inactiveClass != ''){
            $(this).addClass(opts.inactiveClass);
        }else{
            $(this).css({'font-style' : opts.inactiveFontStyle, 'color' : opts.inactiveColour});
        }
        
        $(this).bind('focus', function(){
            if($(this).val() == $(this).attr('title')){
                if(opts.inactiveClass != ''){
                    $(this).removeClass(opts.inactiveClass);
                }else{
                    $(this).css({'font-style' : opts.activeFontStyle, 'color' : opts.activeColour});
                }
                
                $(this).val('');
            }
        });
        
        $(this).bind('blur', function(){
            if($(this).val() == ''){
                if(opts.inactiveClass != ''){
                    $(this).addClass(opts.inactiveClass);
                }else{
                    $(this).css({'font-style' : opts.inactiveFontStyle, 'color' : opts.inactiveColour});
                }
                
                $(this).val($(this).attr('title'));
            }
        });
        
        $(this).blur();
        $(this).val($(this).attr('title'));
        
    });
    
};
{% endhighlight %}


Simply save this as whatever you want (I used **jquery.DefaultText.js**) and start using it now! Docs on how to use can be found [on my Github repo](http://github.com/a1phanumeric/jQuery-Default-Text-Plugin).

## So how do I make a jQuery plugin?

It's really, really simple. It's probably much more simple than you think! The first line is very important:

{% highlight javascript %}
$.fn.DefaultText = function(options) {
{% endhighlight %}

This is where we declare the plugin. `DefaultText` is the name of this plugin. You can name yours whatever you like. As you can see we're sending one parameter **options** to the plugin function. This is a completely optional parameter, but is used to send parameters to our plugin as explained in a moment.


{% highlight javascript %}
    var defaults = {
        inactiveClass:      '',
        inactiveFontStyle:  'italic',
        inactiveColour:     'rgb(204,204,204)',
        activeFontStyle:    'normal',
        activeColour:       'rgb(0,0,0)'
    };
{% endhighlight %}


This is a list of our default parameters. You can see we have a placeholder for `inactiveClass`, and four other parameters. Two for a default inactive state (when the textfield is blurred [no focus on it]) and two for when the user has focused the text field. I coded the plugin like this for two reasons.

1. I like to have a fallback without always having to rely on yet another CSS definition (that many jQuery plugins require)
2. Because this tutorial would be pretty bare without it!

## jQuery Plugin Options

The next line allows us to extend the default parameters (stored in the variable **defaults**) with options passed through the plugin. Basically, this line:

{% highlight javascript %}
var opts = $.extend(defaults, options);
{% endhighlight %}

merges the defaults with any options passed through, for example, like this:

{% highlight javascript %}
$("#textField").DefaultText({inactiveClass: 'myInactiveClass'});
{% endhighlight %}

Any parameters passed through in this way will **overwrite** the defaults as set in the plugin.

## The meat of the plugin

{% highlight javascript %}
$(this).each(function(){
{% endhighlight %}

This is where it all gets going. This line basically initiates the loop through all of the matched elements when we declared the plugin. If we had a form like so:

{% highlight html %}
<form>  
  <input type="text" title="First Input" value="" class="defaultTextInput" /><br />  
  <input type="text" title="Second Input" value="" class="defaultTextInput" /><br />  
  <input type="text" title="Third Input" value="" class="defaultTextInput" /><br />  
  <input type="submit" value="Submit" />  
</form>  
{% endhighlight %}

And initialised the plugin using the following:

{% highlight javascript %}
$(".defaultTextInput").DefaultText();
{% endhighlight %}

Then the loop would match all three of the text inputs, with the class `defaultTextInput`. So for each element we have found, we need to run some JavaScript goodness!

Firstly, we are setting the default state of the inputs, as their unfocused states (in this case, the font will be a light grey, and italicised).

{% highlight javascript %}
        if(opts.inactiveClass != ''){
            $(this).addClass(opts.inactiveClass);
        }else{
            $(this).css({'font-style' : opts.inactiveFontStyle, 'color' : opts.inactiveColour});
        }
{% endhighlight %}

We have an if statement, just so we know whether we're using a class to style the default look/feel of the input, or whether to use the inactive font style and colour. It's important to note here, if the user has chosen to use a class, then that will take precedence over the inactiveFontStyle and inactiveColour.

Next, we bind the focus() function to each of the inputs. Remember, this will bind to EVERY matched element, as we're still inside the `$(this).each()` loop.

{% highlight javascript %}
        $(this).bind('focus', function(){
            if($(this).val() == $(this).attr('title')){
                if(opts.inactiveClass != ''){
                    $(this).removeClass(opts.inactiveClass);
                }else{
                    $(this).css({'font-style' : opts.activeFontStyle, 'color' : opts.activeColour});
                }
                
                $(this).val('');
            }
        });
{% endhighlight %}

This simple function checks to see if the value of the text input matches the title attribute of the input. If it does, it removes the default styling - then sets the value to an empty string - ready for writing into.

In a similar fashion to the above, this next block controls the blur() functionality for each text input:

{% highlight javascript %}
        $(this).bind('blur', function(){
            if($(this).val() == ''){
                if(opts.inactiveClass != ''){
                    $(this).addClass(opts.inactiveClass);
                }else{
                    $(this).css({'font-style' : opts.inactiveFontStyle, 'color' : opts.inactiveColour});
                }
                
                $(this).val($(this).attr('title'));
            }
        });
{% endhighlight %}

This time, we looking to see if the value of the text input is empty. If it is, we put the default styling back on for that text input (the same way we did at the beginning, if the inactiveClass is present - it uses that, otherwise it uses the inactiveFontStyle and inactiveColour values). We then put the default text - stored in the title attribute of the element - as the value of the text input.

{% highlight javascript %}
        $(this).blur();
        $(this).val($(this).attr('title'));
        
    });
    
};
{% endhighlight %}


The final part of this jQuery plugin tutorial is just making sure that every text input is blurred, and has the title attribute as the default text. It's not really necessary at all - it's just there to show how you can run other functionality within the meaty loop. We're then closing the each() loop and closing the function.

## That's it! Easy!

It's really as simple as that! You can now go and author your own uber-1337 jQuery plugins! Don't forget to show me what you've made - I might even feature the best plugins on this site for everyone to use!

- [Download the Plugin Here](http://github.com/a1phanumeric/jQuery-Default-Text-Plugin/archives/master)
- [Documentation / Usage Here](http://github.com/a1phanumeric/jQuery-Default-Text-Plugin/)
- [Demo the jQuery Default Text Plugin](http://edrackham.com/jquery/default_text/)
