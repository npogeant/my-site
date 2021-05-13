---
title: "What’s inside WallStreetBets posts\_? — Topic Modeling and Bi-grams"
description: >-
  The story around GameStop generated a lot of data to work on, whether it be
  posts, remarks, articles, interviews but also financial data…
date: '2021-03-15T16:11:36.456Z'
categories: []
keywords: []
slug: >-
  /@npogeant/whats-inside-wallstreetbets-posts-topic-modeling-and-bi-grams-d00352082e55
---

![](./img/1__2cpuqikqVLS4VHiz7CjKog.jpeg)

The story around **GameStop** generated a lot of data to work on, whether it be posts, remarks, articles, interviews but also financial data with the _GME_ (GameStop index) stock especially.  
As it mainly started with the well known **_WallStreetBets_** subreddit, it is interesting to look at its evolution before getting into the analysis of the content itself.

**_WallStreetBets_** subreddit is focused on finance, the stock market, financial advises… It has now over **9,500,000 subscribers**, more than **83k comments per day** and over **3,700 posts per day**. It is ranked as the second subreddit by daily comments.

If we look at the subscribers daily evolution, we notice the sharp rise at the start of 2021 (GME’s story beginnings), from around 2M subs late December to near 10M early March.

![](../../img/1__gyoNZi2KO3Fivm431BxZMw.png)

The same trend is observed with the number of comments per day during the period. In the end of 2020, the average WallStreetBets comments per day were close to 22k whereas today, it is around 55k.

![](../../img/1__yZ__SoEludPdyMhu7mPYtHA.png)

Now that we introduced the subject, let’s get to the heart of the matter.

For this article, I worked with [Basssall](https://medium.com/u/98f4c4e7cd9f) to establish an analysis on the dataset we will present after. We wanted to examine the way WallStreetBets users interacted during the highlight of the GME story. What was the main topics of discussions ? Which words were the most used ? Do some trends emerged ? Was there different groups of people ?

_Let’s take a look at the data we used…_

### **The Data**

![](../../img/1__ZYLTzAXxIRyzcXF71vZOaQ.jpeg)

We decided to analyzed a dataset from **Kaggle** called : [Reddit WallStreetBets Posts](https://www.kaggle.com/gpreda/reddit-wallstreetsbets-posts) (shared by [Gabriel Preda](https://www.kaggle.com/gpreda)). The data, constituted with more than 40k rows and were collected with [**_praw_**](https://praw.readthedocs.io/en/latest/) (_The Python Reddit API Wrapper_). The posts period go from January 28th to February 15th of 2021 which is exactly the moment were GameStop and the entire market were imploding.  
Our analysis will mainly be on the search of **topics in the content** (topic modeling).

This is what the table looks like :

![](../../img/1__nkjb5t1OwmcOsn69ORBEaw.png)

What will be interesting to look at are titles and bodies of the posts. However, the number of comments and the discussions scores are great information to notice the importance of the subject.

_Let’s go to the analysis…_

### Primary Study

![](../../img/0__tz74x____QUtupBPs5.jpg)

First of all, we wanted to show an overview of the dataset with the main information (except textual data).

#### The posts frequency

What was the content distribution during the period ? We grouped the table by the date to create this visualization :

![](../../img/1__q5PzF3O9rRqcgLhIvhqLCw.png)

January 29th is clearly the most active day on the subreddit with 15,694 unique posts.

#### The comments

Looking at the most commented posts can give an idea of the content we will analyze lately. We also used a groupby function to get the following chart :

![](../../img/1__OTEVtFqzzBorh1koEIAPHA.png)

We quickly understand that January 29th were the most commented day with various posts in the top 10.

### Natural Language Processing

After highlighting some characteristics of the dataset, let’s focus on the main content, the **titles** and **bodies** of the posts. To do that, we firstly cleaned the text with a basic function using regular expression. Then we Tokenized, Lemmatized the text with the NLTK library and removed the stop words to have the most pertinent corpus possible.

body\_df = pd.DataFrame(body\_df.body.apply(lambda x :clean\_text(x)))  
body\_df.body = body\_df.body.apply(lambda x: ' '.join(\[word for word in x if word not in (stop)\]))  
body\_df.body = body\_df.body.apply(lambda x: word\_tokenize(x))  
body\_df.body = body\_df.body.apply(lambda x : \[lemmer.lemmatize(y) for y in x\])  
body\_df.body  = body\_df.body .apply(lambda x: ' '.join(x))

The result of the cleaning leads to a word frequency count that we displayed with a wordcloud (we chose **the bodies of posts** to do this viz) :

![](../../img/1__ywt0qDg__V3ZrEBEKnDcvpQ.png)

_GME_, _Stock_, _Share_ are the most used words in the posts. _Short_, _buy_, _price_ are also present at the top. It is mostly financial words in the context of what was happening with GameStop and the hedge funds.

_Now that we introduced the corpus by looking at the lexicon, let’s go further with the topic modeling…_

#### Topic Modeling

Our goal was to identify some topics and eventually see if groups emerged from the context. Thus, we used the famous **Non-negative matrix factorization** (NMF) method to create our topics. Before that, we tried to build a **latent Dirichlet allocation** (**LDA**) model but the results were different so we decided to keep the NMF construction.

Firstly, we built a TFIDF Vectorizer to remove least frequent tokens and to fit the model. Then we created the NMF model (with 5 topics) and we fitted it with the past matrix. Finally, we created a Dataframe with the components and their importance in the topics.

vectors = TfidfVectorizer(min\_df=50, stop\_words='english')  
X = vectors.fit\_transform(title\_df\_clean.title)  
model = NMF(n\_components=5, random\_state=5)  
model.fit(X)

We did that for the titles and the bodies of the posts but we decided to only show the body ones because it is based on a better and bigger corpus whereas titles are just a sentences.

![](../../img/1__NbpKxjRiV6v0LG21a3Ts5w.png)
undefined

The **first topic** relates to the stock market, it is a general topic with words such as stock/market/price…

![](../../img/1__Ol__YXJ35fuW5Qlt6MJjf5w.png)
undefined

The **second topic** is around the term : buy. It is referred to the buying of GME and other stocks.

![](../../img/1__x9MxQ7tyzCkpcWuRSo9piQ.png)
undefined

The **third topic** is more around holding the position, “to the moon”, winning…

![](../../img/1__7KyLndCEz__LLCZan3VPCNg.png)
undefined

This **fourth topic** is targeting the stocks themselves : AMC, GME, NOK, BB, NAKD…

![](../../img/1__APB1__7EcLvW__ZISdUw8__pg.png)
undefined

An finally, the **last topic** is very interesting because it shows the fight between the reddit users from WSB and trading apps such as the famous **_Robinhood_**.

_Looking for topics in a corpus can explain the context of a content, lt’s see if some bi-grams can be seen and detailed…_

#### Bi-grams frequency

Finally, we wanted to look at the bi-grams in the corpus and eventually find some interesting one. A **bigram** or **digram** _is every sequence of two adjacent elements in a string of tokens, which are typically letters, syllables, or words_ (definition.net).

To create these sequences, we used the library NLTK again with the function _nltk.bigrams()_.

nltk\_tokens = nltk.word\_tokenize('\\n'.join(title\_df\_clean.title))  
titles\_bigrams = \['\_'.join(b) for b in nltk.bigrams(nltk\_tokens)\]

We looked for the bi-grams of titles and bodies, here are the results :

![](../../img/1__jUYvia__IMNEqYl9QgHG75g.png)

We notice **gme\_amc** at the top which are the two stocks that were targeted by the subreddit. Moreover, we see **buy\_gme** (the goal of amateur investors), **hedge\_fund** (the best enemy of users from WallStreetBets) and **still\_holding**/**hold\_line**/**hold\_hold** (the main action that were present during the period).

![](../../img/1__YOpSMtlzwdDLxVkRkJF__2Q.png)

For the bodies, we spot **x\_b** as the most frequent bi-gram. It is followed by **hedge\_fund** and **financial\_advice**. We didn’t really find what x\_b was meaning but the other bi-grams are rather coherent.

### Conclusion

With this analysis, we wanted to understand how the WallStreetBets writers and readers where talking during the story around GameStop and its financial impact. Overall, they were more united than disseminated in multiple groups. We can clearly spot the deviance against Hedge Funds and the trading platforms like Robinhood which was one of the main aspect of the story. GameStop was not really here for its services as a company but mostly as a stock to fight the shorters. The topics we found are telling the importance of the event about the only one subject that was emerging int the corpus.

Thank you for reading the article, I hope you enjoyed it, you can follow me to be informed of my future writings. The entire analysis is available on my [Github](https://github.com/npogeant/wsb-reddit).