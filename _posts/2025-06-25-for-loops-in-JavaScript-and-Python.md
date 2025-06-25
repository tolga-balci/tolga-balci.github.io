---
layout: post
title:  "Loops in Python and JavaScript"
date:  2025-06-25 16:24:12 +0300
categories: JavaScript Python
---

I'm juggling between JavaScript and Python (C for only Arduino) and sometimes I cannot wrap my head around the syntaxes, so I decided that it will be a good idea to create a table to refer to when I'm using different languages.

As a refresher,
-  do is an entry loop, which is the condition is evaluated and the loop is started if the condition is true. 
-  do-while is an exit loop; the loop is executed at least once, even if the condition never evaluates to true. Until the exit condition is met, the loop runs. 

Note here that Python does not have an explicit do-while construct. Therefore we "trick" Python by starting <code>while true</code>, which always evaluates to <code>true</code> and then specify the exit condition.

<!--more-->

<table>
<tr>
<th>Loop</th>
<th>Python</th>
<th>JavaScript</th>
</tr>
<tr>
<td>for</td>
<td>{% highlight python %}for (start_condition, end_condition, step):
    statements{% endhighlight %}</td>
<td>{% highlight javascript %}for (start_condition; end_condition; step)
{statements;} {% endhighlight %}</td>
</tr>
<tr>
<td>for in array<br>(elements only)</td>
<td>{% highlight python %}for item in array:
    statements{% endhighlight %}</td>
<td>{% highlight javascript %}for (let item of array):
{statements;} {% endhighlight %}</td>
</tr>
<tr>
<td>for in array<br>(index and elements)</td>
<td>{% highlight python %} for index, element in enumerate(array):
    statements{% endhighlight %}</td>
<td>{% highlight javascript %} for (let index in array)
{statements array[index];} {% endhighlight %}</td>
</tr>
<tr>
<td>for in objects</td>
<td>same for any iterable</td>
<td>{% highlight javascript %}for (let index of object)
{statements object[index];} {% endhighlight %}</td>
</tr>
<tr>
<td>while</td>
<td>{% highlight python %}start_condition
while end_condition:
    statements
    iteration{% endhighlight %}
</td>
<td>{% highlight javascript %}let start_condition;
while(end_condition){
statements;
iteration;}{% endhighlight %}
</td>
</tr>
<tr>
<td>do while</td>
<td>{% highlight python %}while True:
  statements
  iteration
  if (end_condtion):
    break{% endhighlight %}
</td>
<td>{% highlight javascript %}let start_condition;
do{
statements;
iteration;}
while (end_condition){% endhighlight %}
</td>
</tr>
</table>

