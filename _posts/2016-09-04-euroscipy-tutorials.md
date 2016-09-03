---
layout: post-no-feature
title: "EuroSciPy 2016 Part I: Tutorials"
description: "Summaries and impressions of the tutorials."
categories: articles
date: 2016-09-03
---

I went to EuroSciPy last week. EuroSciPy is an annual Python conference in
Europe with a focus on scientific research. It was held in Erlangen, Germany
this year.

The conference consists of two days of tutorials followed by two days of
the actual conference talks. After the conference there is also one sprint
day. So it's a pretty busy program!

The tutorials consisted of two tracks: an advanced and a beginners track.
I followed the advanced track. During the tutorials you had to download
the accompanying IPython notebook so that you could try the exercises yourself
on your laptop.

## Tutorials day one

### Scikit-learn

Machine learning seems to be the rage these days and at EuroSciPy this is no
exception. Half of the tutorials are related to something about machine or
deep learning. We start off with a tutorial about scikit-learn, a well
known ML library for Python.

As someone who has never done anything with ML before, this was a very helpful
introduction to some of its concepts. With ML you're trying to use existing
data to predict information on new data, but with ML you can also try to find
new patterns in data. Some well known examples of ML applications are:
handwriting recognition, photo recognition (e.g. Facebook tagging using face
recognition).

In the tutorial itself we used scikit-learn to learn about the various
concepts in ML (classification, supervised learning, unsupervised learning,
model validation, etc.). Scikit-learn seems like a very nice to way to learn
ML and it's also pretty easy to make models.

### Advanced Numpy

Numpy provides arrays and various scientific tools and is essentially the
foundation for scientific or high performance computing in Python. This
tutorial was about some of the more advanced and obscure features of Numpy.
We started with a refresher on broadcasting rules for arrays, then structured
arrays which is very useful if you don't want to have to use pandas but still
want some labels in your arrays. Then we learned more about the concept of
"strides" in Numpy. Strides are basically how Numpy does its array indexing
and determine for example the shape of the array. If you want to hack in
Numpy they can also be interesting (and dangerous!).

One example is using the `numpy.lib.stride_tricks.as_strided`. With this
function you can directly manipulate the strides of an array without copying
data, i.e., it returns a *view* of your array. However it does not check
memory block bounds, so it's possible to read/write outside of the allocated
memory of you array if you're not careful. One application of this function is
using it to create a "moving window" through your array.

### Parallel computing, MPI

The first part of the tutorial was about profiling and some examples of (micro)
optimizations in Python. `line_profiler` was a tool I hadn't heard of before,
but using this tool you can profile your code line by line. This seems
incredibly useful to me since I've only ever used cProfile for profiling,
which I find to be complex to use and hard to interpret.

The second part was about the builtin `multiprocessing` library in Python and
the various features it provides to parallelize your code. Another way to
parallellize your code is using MPI. I've only heard of MPI in the context
of Fortran, but apparently MPI (Message Passing Interface) is actually a
*protocol* for sending messages between processes. For Python the library
`mpi4py` is available.

MPI provides various way to group processes together and sending messages
between grouped or individual processes. The most basic is just sending
messages between two processes which is called point-to-point communication.
Other ways to pass messages are: broadcasting (send a message (array) to all
processes), scattering (send chunks of a message (array) to different
processes), etc.

## Tutorials day two

### Keras

Day two of the tutorials started of with another ML related tutorial. The
session was about *deep learning* using the Keras framework. Deep learning
is a type of more sophisticated machine learning using models with multiple
layers. The instructor introduced some basic concepts of artificial neural
networks (ANNs) using some pretty intimidating looking formulas and showed
some examples how to make such a model from first principles. Then he
introduced us to Theano, which is a Python library with its own (not so
Pythonic) syntax, which can be compiled to run on the gpu or cpu.

All of this is pretty complicated if you don't have a ML background (like me).
Thankfully, the Keras library makes things much easier for us. Keras is a
minimalist library that runs on top of either Theano or Tensorflow (another
deep learning framework similar to Theano made by Google). Keras was made to
make it easy and fast to develop deep learning models. The same model in
Theano will generally be much smaller (fewer SLOCs) in Keras, and much more
readable. Thus Keras makes it easier to make simple models as well as complex
models.

The second part of the tutorial was about image recognization using
convolutional neural networks (CNNs), which is basically about using filters
to try to recognize parts of your image. In CNNs also pooling (a.k.a. down
sampling) is used to reduce the size of the layers.

<!--- more stuff here -->

We concluded with the current state of machine learning being still very
young and highly experimental. Even though we've seen some models that
perform pretty well in this session, we still don't now exactly *why* they
perform so well, as there are still no universal rules on how to create a good
model.

### NetworkX

Graphs are basically a collection of nodes and edges. They can be directed
or undirected. Many things can be modeled using graphs. Common examples are
roads, relationships between people, website ranking (Google PageRank).

NetworkX is an open source Python library used to create and analyze graphs.
You can use it compute all kinds of interesting properties, such as shortest
path, number of connections, neighbours, subgraphs. It looks like an
interesting package if you do a lot of that kind of analysis. At my job we
work with numerical flow models which in our case are also graph like things
because they're basically nodes connected by edges, so this might be
interesting for that.

### Advanced pandas

The tutorial instructor is a PhD student in the field of air quality. He does
a lot of time series analysis on which he depends on pandas. pandas is
essentially used when dealing with *tabular* data (think of Excel, SQL table).
You can do all sorts of data analyses with the functionality provided by
pandas. Two of the most powerful techniques are ``groupby`` and ``resample``.
With ``groupby`` you can aggregate results by column. `resample` is very
useful for resampling and aggregating time series data. Furthermore, pandas
provides a `rolling` object which can be used to calculate moving window
statistics on DataFrames, Series, etc.
