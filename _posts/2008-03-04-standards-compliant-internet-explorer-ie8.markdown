---
author: admin
comments: true
date: 2008-03-04 21:36:00
layout: post
slug: standards-compliant-internet-explorer-ie8
title: Standards Compliant Internet Explorer (IE8)
wordpress_id: 16
categories:
- ie8
tags:
- ie8
- web-standards
---

> We’ve decided that IE8 will, by default, interpret web content in the most standards compliant way it can. This decision is a change from what we’ve posted previously.

Don your glad rags and party hats, it's time to celebrate! Microsoft have announced that their new interoperable release of [Internet Explorer (IE8) will be standards compliant by default](http://blogs.msdn.com/ie/archive/2008/03/03/microsoft-s-interoperability-principles-and-ie8.aspx).

Their explaination for change still hints at their stubborness though: _"While we do not believe any current legal requirements would dictate which rendering mode a browser must use, this step clearly removes this question as a potential legal and regulatory issue."_.

### So what does this mean for us as designers?

Don't misconceive me - I'm not primarily a designer, but I do design all sites I build ...in pure CSS. This change will mean that we should know that our designs will render the same in all mainstream browsers such as FireFox, Opera, Safari and of course IE8 (provided we use valid CSS1, 2 or 3 [when fully supported] code). The days of running both FireFox and IE6/7, uploading your new CSS file and hitting F5 to find that the bug you just fixed in IE6/7 has now broken something else layout-wise should now be gone.

IE8 will support standards compliant mode as default, but will be backward compatible with our already 'IE Bug-Fixed' CSS files. So, to save us backtracking to make our previously written CSS files - which have no doubt taken us hours upon hours to write thanks to IE7- rendering modes - we can insert a meta tag into IE8 to set it to "Quirks Mode" for this exact purpose.

### A final note

All in all, I don't have much confidence in Microsoft and the web. This confidence decreases even more after hearing any one of Steve Ballmer's semitars, especially the one about Microsoft winning the web. In my opinion, they never will, but this is a fantastic step in the right direction which leads me to say two things; Well done Microsoft, and two, Let's get used to the days of IE7-.