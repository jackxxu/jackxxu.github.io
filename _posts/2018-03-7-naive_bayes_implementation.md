---
layout: post
title:  "Naive Bayes from Scratch"
date:   2018-03-07 12:50:36 -0400
categories: machine learning, AI, algorithm, naive bayes, classifier
---

## TL;DR

This article provides some background information on Naive Bayes algorithm and details on my design and implementation of it in Ruby.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/Bayes%27_Theorem_MMB_01.jpg/440px-Bayes%27_Theorem_MMB_01.jpg">

Recently, I have spent some time on machine learning. The most basic classifier with ML is Naive Bayes. 

## Naive Bayes Theorem

The classifier is based on [Naive Bayes Theorem], which makes the assumptions that all the features will contribute to the target independently. This assumption is where the epithet *naive* is derived.

[Naive Bayes Theorem]: https://en.wikipedia.org/wiki/Bayes%27_theorem

## Existing implementations

Perhaps the most widely used implementation is in [Python's scikit-learn library]. The interface looks like the following:

```python
>>> from sklearn import datasets
>>> iris = datasets.load_iris()
>>> from sklearn.naive_bayes import GaussianNB
>>> gnb = GaussianNB()
>>> y_pred = gnb.fit(iris.data, iris.target).predict(iris.data)
>>> print("Number of mislabeled points out of a total %d points : %d"
...       % (iris.data.shape[0],(iris.target != y_pred).sum()))
Number of mislabeled points out of a total 150 points : 6
```

There are also implementation in other languages including Ruby.


[Python's scikit-learn library]: http://scikit-learn.org/stable/modules/naive_bayes.html

## My Implementation

Here is the github link: https://github.com/jackxxu/naive_bayes_rb

### Design Principles


1. the interface closely resembles the python [scikit-learn interface].
2. enable model serialization and persistence, so that the model can be reused and even distributed and shared. With the default `MarshalSerializer`, it also allows custom serializer to be plugged in.

### Usage

```ruby
nb = NaiveBayesRb::NaiveBayes.new
train = [[1, 20], [2, 21], [3, 22], [4, 23]] 
target = [1, 0, 1, 0] 
test = [[0, 0], [4, 24]]
predictions = nb.fit(train, target).predict(test) #=> [1, 0] 
@nb.accuracy(prediction, [1, 1]) #=> 50
```
### Model Persistence

```ruby
NaiveBayesRb::NaiveBayes.serializer =       
nb = NaiveBayesRb::NaiveBayes.new
nb.fit(train, target).save('model.pb')
```

### Loading Persisted Model

```ruby
NaiveBayesRb::NaiveBayes.serializer =       
nb = NaiveBayesRb::NaiveBayes.load('model.pb')
```

### Potential Improvements

With what I have currently is a very basic ruby implementation, there are a lot of room for improvements:
1. currently the data is using native ruby array, each element of which is represented using ruby objects. This is fine with small datasets, but for larger dataset, the performance can be better using `narray`-like Ruby constructs.
2. the propability calculation is very primitive. It may be desirable to use logistic probability instead of raw ones. 
3. implement the more optimized naive bayes algorithms
    * Gaussian Naive Bayes
    * Multinomial Naive Bayes
    * Bernoulli Naive Bayes
    * Out-of-core naive Bayes model fitting