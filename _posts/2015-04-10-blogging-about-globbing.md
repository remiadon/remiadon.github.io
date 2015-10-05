---
layout: post
title:  "Blogging about globbing"
date:   2015-10-04
categories: jekyll recursive
tags: [recursive]
---

<h3>Introduction</h3>
Today I will introduce a short and pretty fast way to perform a `Unix globbing` match. <!-- If you are not familiar with making use of glob patterns, <a href="https://en.wikipedia.org/wiki/Glob_(programming)" target="_blank">here</a> is a wiki.\\ -->The implementation bellow is just a simple parser, based on character per character (not exactly, as described later on) treatment.

To be fair, the best way possible to implement globbing is probably using `finite automatas`; cf. further readings for more details about this type of implementation.

The match is a filename match, performed on two given strings : `a pattern and a name to be matched`.

A possible pseudo-code prototype for the main function would be
{% highlight python %}
bool match(str pattern, str name)
{% endhighlight %}

Also, only the two following core features will be discussed here :
<ul>
<li> wildcard characters : <span style="color:#c7254e;">&#42;</span> and <span style="color:#c7254e;">&#63;</span>, respectively star and question mark</li>
<li> characters sets : [...]</li>
</ul>


<h3>Basic characters and question mark</h3>
Going character per character, while the current character in the pattern is not a question mark, and it matches the current character in the string, we move forward.

If the current one is a <span style="color:#c7254e;">&#63;</span>, it actually matches any characters, so we simply move forward, nothing checked.

In Python : 
{% highlight python %}
i = 0
for p in pattern:
	if name[i:] and p is not '?' and p is name[i]:
		i += 1
	if p is '?':
		i += 1
return (i is len(name))		# check name not longer than pattern
{% endhighlight %}

<h3>Star</h3>
The star (not to be confused with the Kleene star), is the crux of our problem.





<h3>Characters sets</h3>





<h4>Further readings</h4>
<ul>
<li><a href="https://swtch.com/~rsc/regexp/regexp1.html" target="_blank">Regular Expression Matching Can Be Simple And Fast</a> by Russ Cox</li>
<li><a href="https://en.wikipedia.org/wiki/Glob_(programming)" target="_blank">Glob</a> wiki</li>
</ul>