---
layout: post
title:  "Analyzing User-Generated Content in Firm-Hosted Social Media Pages"
date:   2017-08-23
---

## Introduction

This page contains resources for my talk "Analyzing User-Generated Content in Firm-Hosted Social Media Pages" at [FARCON 2017](http://minneanalytics.org/farcon-2017/){:target="_blank"}, including links, code snippets, and instructions. Here is the [slides]({{ site.baseurl }}{% link /file/Farcon_slides.pdf %}) of the talk.

## Collecting Data via Facebook API

> **Important**: before collecting data from Facebook and/or any online resources, please make sure that such action is permitted.

### Getting Started

To programmatically collect public information on Facebook, you first need a Facebook developer account. To do so, visit [Facebook for Developer](https://developers.facebook.com/) and create a developer account (typically linked with your regular Facebook account). The account would allow you to create "Apps", i.e., special purpose applications hosted on Facebook, usually to supplement its functionality. However, we use "App" to send requests to collect data.

Every "App" you created has two important pieces of information: an _app id_ and an _app secret_. These two strings can uniquely identify an App and will be used to verify it.

Then, to send requests to Facebook, you also need an _access token_, a long string that grants your permission for your data requests. Visit [Access Token Tool](https://developers.facebook.com/tools/accesstoken/) page to get the "User Token" that corresponds to your App. The user access token you get from this page is called a "short-term token", which expires in an hour or two. You can convert this short-term token to a long-term token that expires in 60 days, by using a python package called `facepy` and the following code snippet:

```python
from facepy import utils
app_id = 'YOUR-APP-ID'
app_secret = 'YOUR-APP-SECRET'
access_token = 'YOUR-SHORT-TERM-ACCESS-TOKEN'

utils.get_extended_access_token(access_token, app_id, app_secret)
```

The function call would return a long-term token and the timestamp when the token would expire.

### Making Requests and Processing Results

Now, we can actually make requests for data using Facebook API. We do so via a python package called `facebook`, which provides easy-to-use functions to make API calls. The `facebook` package [documentation](https://facebook-sdk.readthedocs.io) provides in-depth descriptions of its various functions.

Here is an example of how you can obtain a sample of posts from [Walmart's business page](https://www.facebook.com/walmart/):

```python
import facebook

access_token = 'YOUR-LONG-TERM-ACCESS-TOKEN'
graph = facebook.GraphAPI(access_token)
feed = graph.get_object('/Walmart/feed/', until = '2017-08-23')
```

The output of API requests is usually a `JSON` object, which is structurally similar to a python `dictionary`. One can parse the returned object and get various information.

One intricacy of getting a large amount of data in a continuous fashion is to properly handle `paging`. Suppose you want to collect 1000 posts from Walmart's Facebook page, but Facebook API returns posts in a batch of 100. Then you need to send consecutive requests and specify the position of the next batch. Using the above example, the paging information in the `feed` object is usually in `feed['paging']['next']`, which contains the position of the next batch of posts. In particular, the position of the next batch is specified as the value of "until" parameter. The following code snippet shows how to get the parameter:

```python
# the "next" parameter in paging has structure "...until=XXXXXXX&..."
# the 'until' parameter is the string after 'until=' and before '&'
until_val = feed['paging']['next'].split('until=')[1].split('&')[0]
feed_next = graph.get_object('/Walmart/feed/', until = until_val)
```

### Ethics in Web Scraping

It is important to collect data in an ethical way. Specifically, one should be mindful about two things:

1. Respect any policy that Facebook might have about making API calls. For example, don't make high-frequency API calls or you will be locked out by Facebook (or even worse). Hard-code proper intervals between API calls, e.g., with the help of python's `sleep` function.
2. Protect privacy. Take measures to anonymize your data and never use it for unintended purposes.
3. Do not bypass restriction and try to collect personal, non-public information without consent.

## Topic Modeling: LDA

Latent Dirichlet Allocation (LDA) is a machine learning technique for topic modeling, i.e., discovering salient topics from textual data. The original LDA paper is [here](http://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf), and _Communication of ACM_ has a nice summary article [here](https://cacm.acm.org/magazines/2012/4/147361-probabilistic-topic-models/fulltext).

The python package `gensim` offers easy-to-use functions to do LDA analysis. The following code snippet shows how to run LDA and display topics with `gensim`:

```python
from gensim.models import ldamodel

# “corpus” is a list of documents compatible with gensim format requirement
lda = ldamodel.LdaModel(corpus, num_topics = 5)
# display the top 10 words of each topic
for topic in lda.show_topics(num_words = 10):
    print topic
```

## Amazon Mechanical Turk

[Amazon Mechanical Turk](https://www.mturk.com/mturk/welcome) is an online platform for scalable on-demand workforce that you can leverage to complete a series of "human intelligence tasks", ranging from data labeling, imaging tagging, to running experiments, to crowdsourcing product ideas.

### Turker Demographics

To understand turker demographics, I highly recommend the [mturk-tracker](http://demographics.mturk-tracker.com) website built by NYU professor Panos Ipeirotis, as well as his [blog post](http://www.behind-the-enemy-lines.com/2015/04/demographics-of-mechanical-turk-now.html).

### MTurkR: R Package to Manage MTurk tasks

The R package `MTurkR` provides full fledged capability to make API calls to AMT. Here is its [CRAN page](https://cloud.r-project.org/web/packages/MTurkR/index.html). Below is an example code snippet that shows how you can (1) create a `qualification type` and (2) contact workers in batch

```r
library(MTurkR)
# set your AWS credentials
Sys.setenv(AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY)

# create a qualification type
# "test" parameter can take a qualification test, in XML format
# "test_duration" specifies how long the test lasts
# "retry_delay" specifies how long one can retry the test after a failure
CreateQualificationType(name = "foo", description = "bar", keywords = "a,b,c", retry_delay = 3600, test, test_duration = 1800)

# contact workers in batch
Contactworker(subject = "Hi", msgs = "Welcome to work on my HITs!", workers = workerIDs, batch = TRUE)
```

## Word Embedding with `Word2Vec`

The `Word2Vec` model was developed by researchers at Google. You can find the original paper [here](https://arxiv.org/abs/1301.3781).

The Python library `gensim` provides the capability to train Word2Vec models. Note that you might need additional libraries such as `cython` and `OpenBLAS` to achieve computational efficiency.

Before training the `Word2Vec` model, some text pre-processing steps may be necessary, such as parsing a post into individual sentences, removing/transforming certain words etc., depending on the specific need of your task.

The following code snippet shows how to train a `Word2Vec` model:

```python
import gensim
# "sg" specifies whether to use skip gram or continuous bag-of-words algorithm
# "size" specifies dimension of word embeddings
# "window" specifies number of surrounding words to take into account
model = gensim.models.Word2Vec(sentences, sg = 1, size = 300, window = 10)

# model persistence
model.save("DIRECTORY-TO-SAVE")

# use model to get word similarity
# 1. get most similar words to "pet"
model.most_similar("pet")
# 2. get similarity score between two words
model.similarity('dog', 'cat')
```

## Recurrent Neural Network

`TensorFlow` is a popular and sophisticated Python library to train neural network models. Specifically, we can use `TensorFlowRNNClassifier` to train a recurrent neural network to predict post content categories.

The following code snippet shows how to initialize a classifier, train it, and use it to make predictions:

```python
from tensorflow.contrib import learn

# RNN has a lot of hyperparameters, here are just a few
# 1. rnn_size - typically the size of your embedding
# 2. n_classes: how many categories
# 3. cell_type: type of neural cells
# 4. num_layers: depth of the network
# 5. optimizer: which optimization algorithm to use
classifier = learn.TensorFlowRNNClassifier(rnn_size = EMBEDDING_SIZE, n_classes, cell_type, num_layers, optimizer)

# train the classifier on training data
classifier.fit(X_train, y_train)

# make predictions
classifier.predict(X_unlabeled)
```
