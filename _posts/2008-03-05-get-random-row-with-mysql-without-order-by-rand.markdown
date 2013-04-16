---
author: admin
comments: true
date: 2008-03-05 12:12:24
layout: post
slug: get-random-row-with-mysql-without-order-by-rand
title: Get Random Row with MySQL Without ORDER BY RAND()
wordpress_id: 17
categories:
- mysql
tags:
- fast
- mysql
- random row
---

This is an update to a [previous post of mine]({{ site.url }}/mysql/get-random-row-with-mysql/) which uses the RAND() method. Using the following code, you can retrieve a random row much, much faster (MySQL 4.1.x/5.0.x), with thanks to Jan Kneschke:

{% highlight mysql %}
SELECT <COLUMN> FROM <TABLE> AS r1
JOIN (SELECT ROUND(
  RAND( ) * (
    SELECT MAX( id ) FROM <TABLE>)
  ) AS id
) AS r2
WHERE r1.id >= r2.id
ORDER BY r1.id ASC
LIMIT 1;
{% endhighlight %}

Replace:

* **COLUMN** with the name of the column(s) you wish to retrieve
* **TABLE** with the name of the table you wish to retrieve the data from

I've tested this with a table with over 660,000 records, and got a response in 0.0200 seconds, whereas with ORDER BY RAND() i got a response in 2.1599 seconds.

#### Total Number of Rows:

![Cardinality]({{ site.url }}/images/posts/num-rows.gif)

#### Old Method:

![Old Method]({{ site.url }}/images/posts/faster-rand-mysql-old-query.gif)

#### New Method:

![New Method]({{ site.url }}/images/posts/faster-rand-mysql-new-query.gif)