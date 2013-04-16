---
author: admin
comments: true
date: 2009-01-16 13:38:18
layout: post
slug: php-date-picker
title: PHP Date Picker
wordpress_id: 25
categories:
- php
tags:
- php
- code
---

This is a simple to use script that can be called at anytime to insert a date picker into your form.

### Usage

Just make a call to 
{% highlight php %}
<?php
echo DatePicker();
?>
{% endhighlight %}

wherever you want the date picker to show. If you want to use it multiple times, just call it multiple times like 

{% highlight php %}
<?php
echo DatePicker();
echo DatePicker();
?>
{% endhighlight %}

### The Code

{% highlight php %}
<?php
$iDatePickerCounter = '';		// Used for having multiple date pickers
														// comment it out if you are only going to
														// use ONE datepicker call on this page.
														// (It will make the form element names
														// nicer to work with).
$sYearName 			= 'Year';		// Base name for the Year form element
$sMonthName 		= 'Month';	// Base name for the Month form element
$sDayName 			= 'Day';		// Base name for the Day form element

$iFromYear			= 1985;			// Starting Year
$iToYear				= 2030;			// Ending Year

function DatePicker(){
	// Call up the global variables
	global $iDatePickerCounter,
		   $sYearName,
		   $sMonthName,
		   $sDayName,
		   $iFromYear,
		   $iToYear;
	
	// Set up some base variables
	$sPostFix = '';
	$sNL = "\r\n";
	
	if(isset($iDatePickerCounter)){
		$iDatePickerCounter++;
		$sPostFix = '_' . $iDatePickerCounter;
	}
	
	// Start the coding of the SELECT areas
	$sYearDropDown 	= '' . $sNL;
	$sMonthDropDown = '' . $sNL;
	$sDayDropDown 	= '' . $sNL;
	
	// Year loop
	for($i = $iFromYear; $i <= $iToYear; $i++){
		$sYearDropDown .= "\t" . '' . $sNL;
	}
	$sYearDropDown .= '' . $sNL . $sNL;
	
	// Month Loop
	$sDummyDate = '2008-01-01';
	for($i = 0; $i < 12; $i++){
		$sMonthDropDown .= "\t" .'' . $sNL;
	}
	$sMonthDropDown .= '' . $sNL . $sNL;
	
	// Day loop
	for($i = 1; $i <= 31; $i++){
		$sDayDropDown .= "\t" .'' . $sNL;
	}
	$sDayDropDown .= '' . $sNL . $sNL;
	
	return $sYearDropDown . $sMonthDropDown . $sDayDropDown;
}
?>
{% endhighlight %}

