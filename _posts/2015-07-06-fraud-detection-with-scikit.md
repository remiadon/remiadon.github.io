---
layout: post
title:  "fraud detection with scikit"
date:   2015-06-07
categories: jekyll python
tags: [python, machine-learning]
---

<h2>Introduction</h2>
<h3>The case of study</h3>
This year I was an intern for a market place,
and this company recently acquired anothter website which is as one could say an AirBnB like.
<!-- After a few month of work in the team I learned that some csv files were oftenly generated, and looking closer there is one about hostings.   -->
After a few month of work in the company I learned that an Excel file of all the hostings was oftenly generated.

This csv basically is an exctract of the hosting table in the database, and contains numerous and quite `meaningfull informations`, such as the type of the hosting in question (room / flat / house), its surface area, its price, both per night and per week, its deposit amount, the cancel conditions, its grades and the comments of the users about it, the owner's email adress and the number of photos he or she has provided, the total number of reservations, etc...  
Last but not least the fact is that `cases of evidence of fraud where clearly tagged` -- a key word was present at the beginning of the email adress.

Needless to say; in our case by evidence I mean they had been manually checked by our staff through what I'd call an `empirical and repetitive process`.  

It quickly occured to my project manager and myself that it is quite a pleasant case for a machine learning approach.

In this post I'll explain my approch quite from scratch, so that newbies (which I nearly am) can understand everything.
<br>


<h3>What exactly are week getting into ?</h3>
Here we are dealing with `supervised learning`. Why ? Simply because we already know a non-negligeable part of the answer.  We have labels in our possessions.
What we are doing is actually separating cases of fraud from legally acceptable cases; to succeed we will need a `classifier`, which is to be differenciate from a regresser.<br>
The difference is in the maths in there : while a classifier come as an answer to a discrete problem, a regresser is fitted for a continuous problem.  
In our case we clearly need a classifier : we have two and only two static possibilities, fraud or non fraud, 1 or 0.
There is no inbetween.

In machine learning we have many models to solve the given(s) problem(s), they can be either used separatly or piped together.
In this tutorial we are going to use a model called `Support Vector Machine` (<a href="https://en.wikipedia.org/wiki/Support_vector_machine" target="_blank">here</a> is the wiki).
If we combine Support Vector and what is written on the paragraph above we end up understanding we actually need a 'Support Vector Classifier' : a classifier based on support vector machine.  
How lucky we are, and here is the point, all the models and the methods that we need are provided by the <a href="https://www.scikit-learn.com" target="_blank">scikit-learn</a> library.

So here we are, we are going to tackled some of the main machine learning techniques -- including the infamous `cross-validation` technique to get good results from our dataset (our csv), in order to be able to classify the future hosting by making predictions.

<!-- "Talk is cheap, show me the code"  
If you are new to machine learning, at this point you better ty your shoes. -->

<h2>1. Preprocessing</h2>
The aim of the preprocessing part is simple : we want to get from our source file to an `exploitable` (well, scikit readable) array.
It does not take much time digging into the scikit documentation to find out scikit treats with `numpy arrays`.

As mentionned before, my source file is a csv containing all the hostings. The first step is then to define a mapping between the columns of my excel file and the features of my model...

<h3>Mapping</h3>
I simply define a python dictionnary, and order it by key for better usabality, eg :

{% highlight python %}
from collections import OrderedDict

m = {
	1 : 'status',
	2 : 'type',
	...
	...
}

mapping = OrderedDict(sorted(m.items(), key=lambda t: t[0]))
{% endhighlight %}

<h3>is_fraud and csv_to_tab</h3>
Before iterating through the csv, we have to write a function that returns a boolean from a reservation : this is how we "supervise" our learning.
If in your case, a "fraud" tag was included in the email address.
{% highlight python %}
def is_fraud(mail):
	try:
		return float(not mail.lower().find('fraud') == -1)
	except AttributeError as e:
		print '%s\n' % e
		# return 0.0
{% endhighlight %}

Then we can simply iterate through the file in order to get our numpy array.
Here is an example :
{% highlight python %}
def csv_to_tab(reader, mapping):
	X = y = []
	for row in reader:
		mail = get(row, 35)
		floats = get_vals(row, mapping)
		X = np.append(X, floats)
		y = np.append(y, is_fraud(mail))

	assert (len(X) == len(y))
	return (X,y)
{% endhighlight %}

<!-- <h3>Format</h3>
At this point we simply have floats in a numpy array, but  -->