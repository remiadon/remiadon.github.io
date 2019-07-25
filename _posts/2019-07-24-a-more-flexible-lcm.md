---
layout: post
title:  "A more flexible LCM"
date:   2019-07-24
categories: jekyll pattern-mining
tags: [pattern-mining]
---

## Context
For the last 2 years I have been reading papers about pattern minining. Although this field of data mining has only received little attention in the last decades, as compared to Machine Learning for example, I found the existing algorithms could bring a lot of value on the table in practice.

One of the main algorithm for generic purpose pattern mining is LCM, short for `Linear Closed itemset Miner`
While reading about this algorithm in the papers presented by [Alexandre Termier](http://people.irisa.fr/Alexandre.Termier/) , there seemd to be practial limitations ruling the existing algorithms.

[This paper's preliminaries section](https://tel.archives-ouvertes.fr/tel-01006195/document) gives a good overview of the principles that are common to almost every (re)-implementation of LCM you can find.
Amongst the core concepts is what is called a `reduced dataset`. Basicaly a reduced dataset is nothing more than a slice a slice of the original dataset; that is to say of subset of the original transactions.

------------

As an example, imagine we are trying to find the `frequent closed patterns` in a transactional database, this transactional database can be stored in datastructure like a `pandas.Series`, like so:

{% highlight python %}
import pandas as pd
db = pd.Series([
    [1, 2, 3, 4, 5, 6],
    [2, 3, 5],
    [2, 5],
    [1, 2, 4, 5, 6],
    [2, 4],
    [1, 4, 6],
    [3, 4, 6],
])
{% endhighlight %}

----------

In practice, being able to acces a `reduced dataset` means having access to the original transactional dataset via `positional indexing`. LCM-like algorithms keep an inner datastructure to get access to the part of the dataset containing a specific item.
The corresponding inner struct for the database above is the following:

{% highlight python %}
item_to_tids = {
    1: {0, 3, 5},
    2: {0, 1, 2, 3, 4},
    3: {0, 1, 6},
    4: {0, 3, 4, 5, 6},
    5: {0, 1, 2, 3},
    6: {0, 3, 5, 6}
 })
{% endhighlight %}

-------------

We simply keep track of every transaction id for every item.
Accessing all transactions containing item 4 become something like
{% highlight python %}
db.iloc[item_to_tids[4]]
0    [1, 2, 3, 4, 5, 6]
3       [1, 2, 4, 5, 6]
4                [2, 4]
5             [1, 4, 6]
6             [3, 4, 6]
{% endhighlight %}

------------

Unfortunately, If we implement this algorithm keeping the dependency with pandas, we will have many calls to `.iloc`.
What the original LCM implementation does is it access a reduced dataset before counting occurences of each item in this reduced dataset, and this operation is done many times ...

For example, if we just discovered the 2-length itemset {2, 4} and we want to check for its closeness (*first parent test*), we have to do something like

{% highlight python %}
new_tids = item_to_tids[2] & item_to_tids[4]
reduced_db = db.iloc[new_tids]  # reduced db contains all transactions containing both 2 and 4
new_counts = Counter()
for transaction in recuced_db:
    new_counts.update(transaction)
{% endhighlight %}

-----------------------

## The state of research
When they write papers, researches make lots of assumptions. While these assumptions may not seem really binding at first sight, in practice some can make your life a lot harder.

As I said, the original definition of `LCM assumes you can access subsets of the original data very quickly given a set of positions`. This may be true if all of it fits in a small pd.Series (as seen above), but in modern applications, this is not always easy to ensure, because your data may be

1. `Too big to hold in memory`. Or even worse, your data may be accessible for a limited time lapse, as in streaming workflows Having access to the original dataset during the entire run of the algorithm means extra memory costs which are not needed.

2. `distributed` (or partiionned), positional indexing is not something easy to ensure. As an example, the [dask API](https://docs.dask.org/en/latest/dataframe-indexing.html#positional-indexing) voluntarily bans positional indexing over rows.

In addition to this the above pseudo code emphasize how much we need of Counters. We have to count items everytime we dig into a subset, again and again ...

There may be a better way

----------------------

## Now big questions
In the next series of post I will try to design a new algorithm, based on previous works on LCM.

Considering the painpoints I have mentioned, the ideal algorithm should :

1. `Only need one pass over the original dataset`. Transactions could be both inserted and removed from the inner datastructures (mainly the *item_to_tids* mapping). This way we could keep track of freshly obtained data, with only the information we need.

2. Do the entire mining with only knowledge about the dedicated datastructure, thus `not requiring awareness of the original dataset`.

3. Be `generic in terms of data-type`. Most LCM implementations work with numeric types. But actually the only important constraint to ensure is having `comparable items (i.e lexicographical order)`. In other words items can be of any type as long as we are able to keep them in a sorted data structure.

4. Of course, keep running times and memory consumption competing with standards pattern mining algorithms

While all this seems a little ambitious, there is pretty much no doubt it's an interesting challenge to take on
