---
layout: post
title:  "Analyzing User-Generated Content in Firm-Hosted Social Media Pages"
date:   2017-08-23
---

## Introduction

This page contains resources for my talk "Analyzing User-Generated Content in Firm-Hosted Social Media Pages" at FARCON 2017, including links, code snippets, and instructions.

## Collecting Data via Facebook API

**Important**: before collecting data from Facebook and/or any online resources, please make sure that such action is permitted.

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


## Amazon Mechanical Turk



## Word Embedding



## Recurrent Neural Network



{% highlight python %}
import facebook
for i in range(10):
	print i
{% endhighlight %}
