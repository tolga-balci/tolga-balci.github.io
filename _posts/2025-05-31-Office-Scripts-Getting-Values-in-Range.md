---
layout: post
title:  "Office Scripts: Getting Values in Range"
date:   2025-05-30 09:48:38 +0300
description: "Office Scripts returns incorrect result with .getValues() in a specific scenario. I explain what has happened and how I solved it."
categories: Office-Scripts
---

I'm working on an Office Script on Microsoft Excel and trying to understand what is happening.

I have an Excel workbook where I am trying to get the values in a range to an array. The range is 98 columns with varying number of rows, and contains duplicate values accross the cells. The range contains 2248 values, and I need to reduce them to unique values to further use in the code.

The steps are:
1. Get all the values in the range to an array,
2. Make sure the array contains only the unique values.


**Step 1**

The most efficient way to get the values in this range would be using the `getValues()` function on the range:
<!--more-->
{% highlight typescript %}
// First row contains values, hence the offset range:
dataRange = dataSheet.getUsedRange().getOffsetRange(1,0)
allValuesArr = dataRange.getValues()
{% endhighlight %}

The problem that I had with this piece of code was,  whatever the number of rows and columns are present in the range, the array length was always 101. Checking the daataRange with `dataRange.getAddress()` always returned the correct address. In my case the range is A2:BF102.

The interesting this was, when I ran a for loop in the dataRange, I got the values correctly:

{% highlight typescript %}
for (let colCount = 1; colCount <= dataSheetColumnCount; colCount++) {
    for (let rowCount = 1; rowCount < dataSheetRowCount; rowCount++) {
        let cellValue = dataRange.getCell(rowCount, colCount).getValue()?.toString();
        allValuesArr.push(cellValue)
{% endhighlight %}

I digged in and out of many debugging scenarios, trying to understand the discrepancy between the built-in getValues() function. Once I had run out of ideas, I hit Stack Overflow and picked [Taller](https://stackoverflow.com/users/22192445/taller)'s brains (he was so kind, helpful and patient - see [Stack Overflow post](https://stackoverflow.com/questions/79618114/office-script-different-array-length-results-between-getvalues-and-looping).)

Nothing seemed to work, except one morning I realized that there could be an issue with the worksheet that the user provided, and which we are working on.

Boom! It was exactly what I thought. The user had inconsistent formatting in the cells, had some disjointed cells throughout the worksheet and various cell background fills. This was getting the script confused and it was only getting the first 101 values in the A44:E44 range. Once I cleared all the formatting and ran the `dataRange.getValues()` I got all the values on the sheet without any hiccups.


**Step 2**

The next step is to get the unique values. There are a lot of options/algorithms to achieve that, but for arrays my preference is to use the `Set` data type where I can. I find this to be way efficient than other methods, as the `Set` data type is a built-in data type in the languages that I use (Python, VBA, JavaScript, Office Scripts) and therefore already optimized.

The line below creates a new set from the allValuesArr, effectively removing all duplicates, and then converts it back to an array with the function `Array.from`.

{% highlight typescript %}
// Following the variable names from above
uniqueValuesArr = Array.from(new Set(allValuesArr))
{% endhighlight %}

The conversions are almost done on the fly, does not need any algorithms/code changes and simple enough to write/understand.
