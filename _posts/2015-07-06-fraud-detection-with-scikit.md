---
layout: post
title:  "fraud detection with scikit"
date:   2015-06-07
categories: jekyll python
tags: [python, machine-learning]
---

<h2>Introdiction</h2>
<h3>The case of study</h3>
This year I was an intern for a market place called <a href="www.e-loue.com" target="_blank">e-loue</a>,  
and this company recently acquired anothter website : (<a href="www.sejourning.com" target="_blank">www.sejourning.com</a>), which is as one could say an AirBnB like.
<br>
After a few month of work in the team I learned that some csv files were oftenly generated, and looking closer there is one about hostings.  

This csv basically is an exctract of the hosting table in the database, and contains numerous and quite `meaningfull informations`, such as the type of the hosting in question (room / flat / house), its surface area, its price, both per night and per week, its deposit amount, the cancel conditions, its grades and the comments of the users about it, the owner's email adress and the number of photos he or she has provided, the total number of reservations, etc ...  
Last but not least the fact is that `cases of evidence of fraud where clearly tagged` -- a key word was present at the beginning of the email adress.

Needless to say; in our case by evidence I mean they had been manually checked by our staff through a `empirical and repetitive process`.  

It quickly occured to my project manager and myself that it is quite a pleasant case for a machine learning approach.
<br>


<h3>But wait, what exactly are week getting into ?</h3>
Here we are dealing with `supervised learning`.  Why ? Simply because we already know a non-negligeable part of the answer.  We have labels in our possessions.
What we are doing is actually separating cases of fraud from legally acceptable cases, to succeed we will need a `classifier`, which is to be differenciate from a regresser.
The difference is quite in the maths in there : while a classifier come as an answer to a discrete problem, a regresser is fitted for a continuous problem.  
In our case we clearly need a classifier : we have two and only two static possibilities, fraud or non fraud, 1 or 0.
There is no inbetween.

In machine learning we have many models to solve the given(s) problem(s), they can be either used separatly or piped together.
In this tutorial we are going to use a model called `Support Vector Machine`, I let yo reffer google for that.
If we combine Support Vector and what is written on the paragraph above we end up understanding we actually need a 'Support Vector Classifier' : a classifier based on support vector machine.  
How lucky we are, and here is the point, all the models and the methods that we need are provided by the <a href="https://www.scikit-learn.com" target="_blank">scikit-learn</a> library.

So here we are, we are going to tackled some of the main machine learning techniques -- including the infamous `cross-validation` technique to get good results from our dataset (our csv), in order to be able to classify the future hosting by making predictions.

"Talk is cheap, show me the code"  
If you are new to machine learning, at this point you better ty your shoes.


<!-- {% highlight python %}
from sklearn import bitch
def youlose():
	break
{% endhighlight %} -->