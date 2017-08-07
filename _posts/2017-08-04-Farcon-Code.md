---
layout: post
title:  "Analyzing User-Generated Content in Firm-Hosted Social Media Pages"
date:   2017-08-23
---

## Introduction

This page contains resources for my talk "Analyzing User-Generated Content in Firm-Hosted Social Media Pages" at FARCON 2017, including links, code snippets, and instructions.

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

Here is an example of how you can obtain a sample of posts from Walmart's business page:

```python
import facebook

access_token = 'YOUR-LONG-TERM-ACCESS-TOKEN'
graph = facebook.GraphAPI(access_token)
feed = graph.get_object('/Walmart/feed/', until = '2017-08-23')
```

The output of API requests is usually a `JSON` object, which is structurally similar to a python `dictionary`. One can parse the returned object and get various information.

One intricacy of getting a large amount of data in a continuous fashion is to properly handle `paging`. Suppose you want to collect 1000 posts from Walmart's Facebook page, but Facebook API returns posts in a batch of 100. Then you need to send consecutive requests and specify the position of the next batch. Using the above example, the paging information in the `feed` object is usually in `feed['paging']['next']`, which contains the position of the "next" batch of posts.

### Ethics in Web Scraping

It is important to collect data in an ethical way. Specifically, one should be mindful about two things:

1. Respect any policy that Facebook might have about making API calls. For example, don't make high-frequency API calls or you will be locked out by Facebook (or even worse). Hard-code proper intervals between API calls, e.g., with the help of python's `sleep` function.
2. Protect privacy. Take measures to anonymize your data and never use it for unintended purposes.
3. Do not bypass restriction and try to collect personal, non-public information without consent.

## Amazon Mechanical Turk



## Word Embedding



## Recurrent Neural Network
