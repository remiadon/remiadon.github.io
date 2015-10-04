---
layout: post
title:  "recursive globbing"
date:   2015-10-04
categories: jekyll recursive
tags: [recursive]
---

<h3>Introduction</h3>
Today I will introduce a short and pretty fast way to perform a globbing match.
Globbing could better be implemented using finite automatas; check the further readings for more details about it.

If you are not familiar with making use of glob patterns, <a href="https://en.wikipedia.org/wiki/Glob_(programming)" target="_blank">here</a> is a wiki.\\
The implementation bellow is just a simple parser, based on character per character per character treatment.\\
Only the two following core features will be discussed here :
<ul>
<li> wildcard characters : * and ? </li>
<li> characters sets : [...]</li>
</ul>



<h3>wildcards</h3>
<h3>characters sets</h3>





<h4>Further Readings</h4>
<a href="https://swtch.com/~rsc/regexp/regexp1.html" target="_blank">Regular Expression Matching Can Be Simple And Fast</a> by Russ Cox
