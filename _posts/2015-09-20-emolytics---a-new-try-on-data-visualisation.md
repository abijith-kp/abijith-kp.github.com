---
layout: post
title: "Emolytics - A new try on data visualisation"
description: ""
category: old_blog
tags: [data mining, nlp, sklearn, matplotlib, basemap, twitter]
---

I was actually interested in how [Natural Language Programming][1] was [working][8]
on and was trying to learn how the actual work flow happens. The current social
networking sites would be one the best places to get some interesting
informations from. Then the idea stoke me. Why don't I jus combine them??

The idea is basically simple. As a preprocessing, create a classifier to
classify the tweets as positive or negative baised ones(basically do [sentiment
analysis][2] on the tweets). The using the [twitter streaming API][9], iterate through
each incomming tweets, classify them into their respective classes. The
location details are also taken from the metadata of the incomming tweets. The
next step comes the visualisation part. Using the above calculated details like
location and sentiment class.

In the first prototype that I had used [TfidfVectorizer][3] for [feature extraction][4],
[MultinomialNB][5] for binary classification. SKlearn library was used for both these
tasks. For accessing twitter api, I made use of the [Tweepy][10] python library. The
visualisation part was done using the [basemap][6] extension for [matplotlib][7].

Note 1: I am not sure wheather this task was done before by someone else. This is
only meant as a hobby project.

Note 2: I'll update this post soon.

#### UPDATE

I had restarted working on this earlier last week. My first aim to make it into
a webapp. Since before I had a little exposure and since this will only be a
small application I preffered using Flask for development. In the intial phase
of development, I kept all the other components as such and just ported the UI
from matplotlib to flask platform.

Later I decided to move from using regular threading to redis queues for job
processing tasks. Instead of working directly with mongodb wrappers, this time I
went with using SQLAlchemy and Sqlite DB. While using PyMongo(python wrapper for
mongodb), there were many things that was to be done manually. But when using
SQLAlchemy all the database migrations and all are handled by the module. All
the models were were defined and the base class used was "Model" from SQLAlchemy
module. This will help in migrating all the model classes to database tables.


I am planning on writting a more detailed post on how Emolytics works. I guess
writting that post will also help me in reviewing what I have done till date.


[1]: https://en.wikipedia.org/wiki/Natural_language_processing "NLP"

[2]: https://en.wikipedia.org/wiki/Sentiment_analysis "Sentiment Analysis"

[3]: http://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html "TfidfVectorizer"

[4]: http://scikit-learn.org/stable/modules/feature_extraction.html#feature-extraction "Feature Extraction"

[5]: http://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.MultinomialNB.html "MultinomialNB"

[6]: http://matplotlib.org/basemap/ "basemap"

[7]: http://matplotlib.org/ "matplotlib"

[8]: https://github.com/abijith-kp/DataMining_NLP_AI "github.com/abijith-kp/DataMining_NLP_AI"

[9]: https://dev.twitter.com/streaming/overview

[10]: http://www.tweepy.org/
