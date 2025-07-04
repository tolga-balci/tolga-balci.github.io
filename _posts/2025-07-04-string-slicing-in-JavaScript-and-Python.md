---
layout: post
title:  "String Slicing in JavaScript and Python"
date:  2025-07-04 09:32:13 +0300
categories: JavaScript Python
---

I am posting this so that you don't scratch your head with string slicing in JavaScript like me if you are coming from a Python background.

I spent some gray cells today while working on a capitalize function in JavaScript. The function simply takes a word as an input and capitalizes it.

I thought that since a string is an iterable, I could slice it with the indexes. So, I can get index 0, convert it to upper case, and then add <code>[1:]</code> as the remainder of the word.

Unfortunately that trick from Python did not work in JavaScript; Python's <code>[start_index:end_index]</code> does not work. JavaScript only allows to access one index, not a range. 

Therefore, the JavaScript method that we need to use is the <code>slice(start_index, end_index)</code>. But since JavaScript does not figure out the <code>end_index</code> automatically, we need to get it by the <code>string.length</code> method.


{% highlight javascript %}
function capitalize(word){
return word[0].toUpperCase() + word.slice(1, word.length);
}
{% endhighlight %}

