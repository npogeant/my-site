---
title: The Amazon Stock vs Twitter in 2019
description: >-
  As a graduate in Finance and a current student in Data Science, I decided to
  look at a not super famous field in Finance called…
date: '2021-01-16T09:20:27.733Z'
categories: []
keywords: []
slug: /@npogeant/the-amazon-stock-vs-twitter-in-2019-931c88831fc1
---

As a graduate in Finance and a current student in Data Science, I decided to look at a not super famous field in Finance called “behavioral finance”.  
This field tries to understand and explain why investors behave in a certain way depending on the situation. The main goal of this field is to show that the Financial Standard theory based on the fact that financial individuals take decisions from a rational perspective is not what we can see.

The hypothesis I will focus on is the “mass behavior effect” on the sentiment of investors. Do investors are influenced by the mass ? Are they all taking the same decisions ? Does the influence from some “mass media” can change the behavior of investors ?

I decided to look at these questions with my analysis. To do so, I chose Twitter to be the reflect of the thinking of investors. By tweets, you can take out what people are felling, experiencing on a subject or about their life.  
Machine learning and data science make it possible by the numerous algorithms that has been created from statistical and mathematical theory.

I will start by introducing the data I selected and the method I chose to get the feeling from the tweets. Then, I will analyze the data, output what’s inside it and finally do the sentiment analysis.

### **The data**

I decided to get my own data to create my own analysis and be free to look at what I wanted to with the right information. My analysis aims to understand the financial sentiment of Amazon on Twitter. By financial sentiment, I mean the feeling of the value of $AMZN, Amazon’s Stock. To do that, I scrapped tweets for all the year of 2019 with the keyword “$AMZN”, which is like a hashtag for a stock.

I did that monthly to have smaller files which lead to this code example for the import of January 2019.

```python
import pandas as pd  
import json

jan2019 = pd.DataFrame(\[json.loads(line) for line in open(r'../Amazon Tweets/AMZNjan2019.json', 'r')\])

jan2019.head(5)
```

![](../../img/1__40ue68IKk9Q3h7Tob8yemQ.png)

Each dataframe has many useless columns for the analysis, that I dropped, such as the “id”, the different links associated with the tweet… I also decided to merge each month into one big dataframe to be able to analyze it in the best way. I get the number of **185 303 tweets** for the year of 2019.

A particularity of the data I get is the fact that information about the user from each tweet is stocked in a column in the main dataframe (the column “user”). Then I created 12 other table from the these information which will be analyze later.

### **The method**

I wanted to do a sentiment analysis which is “**the process of determining whether a piece of writing is positive, negative or neutral**” (_Lexalytics_), It uses Natural Language Processing and Machine Learning to be able to recognize the polarity of a sentence.

As my corpus is based of financial tweets, using a NLP model created from a text that is far from the vocabulary of finance would have been a problem. Then, I looked for a model that would be built from a large financial corpus and I found **FinBERT**.

[**FinBERT: Financial Sentiment Analysis with BERT**  
_Doğu Tan Aracı, Zulkuf Genc_medium.com](https://medium.com/prosus-ai-tech-blog/finbert-financial-sentiment-analysis-with-bert-b277a3607101 "https://medium.com/prosus-ai-tech-blog/finbert-financial-sentiment-analysis-with-bert-b277a3607101")[](https://medium.com/prosus-ai-tech-blog/finbert-financial-sentiment-analysis-with-bert-b277a3607101)

**FinBERT** is a pre-trained model for Financial Sentiment Analysis created from a language representation model called BERT. BERT allow the pre-trained model to be fine-tuned and trained. The team behind FinBERT used BERT pre-trained model and trained it on a Financial corpus : [Reuters TRC2](https://trec.nist.gov/data/reuters/reuters.html).

_Let’s now look at the tweets and do the analysis…_

### **I - Amazon Tweets for 2019**

As I said before, I only looked at tweets with $AMZN to stay relevant with what I wanted to do. If we pay attention at the number of tweets by day, we get that output.
```python
import plotly.express as px

AMZN2019\_tweets = AMZN2019.groupby(\['date', 'month'\]\['username'\].count().reset\_index()  
AMZN2019\_tweets.columns = \["Date", "Month", "Number"\]

fig = px.bar(AMZN2019\_tweets, x="Date", y="Number", color="Month", title="Tweets on AMZN per day in 2019", facet\_col="Month", color\_discrete\_sequence=px.colors.qualitative.Antique)  
fig.update\_xaxes(matches=None)  
fig.update\_layout(showlegend=False, plot\_bgcolor='white', yaxis=dict(title=''), margin=dict(autoexpand=False, l=100, r=20, t=110,))  
fig.update(layout=dict(title=dict(x=0.5)))  
fig.update\_xaxes(visible=False)  
fig.show()
```
![](../../img/1__w7Zw1BZnWvakwGK1rUMubw.jpeg)

We can see a trend for each month, the day with the least tweets are the weekends because the Stock Exchange is closed. However, some days are clearly different from the others with more than 1500 tweets.

If we look at the 10th most tweets days, we get that :

#A Dataframe sorting days by tweets  
Ten\_mosttweetsdays = pd.DataFrame(AMZN2019\_most\_tweets.head(10))  
Ten\_mosttweetsdays.sort\_values(by="Number", inplace=True, ascending=True)

![](../../img/1__c1CCMfLiEQ9b__w6GCYjNnQ.png)

The day with the most tweets on $AMZN in 2019 is October the 24th. It is easy to understand why. On that day Amazon made a conference call to discuss its third quarter 2019 financial results (source 1 : _press.aboutamazon.com_ and _nytimes_). The second one is the next day of that.  
The 3rd day with the most tweets (February the 1st) is also explainable by an announce. Indeed, the day before, January the 31st, Amazon published an increased of Sales in the Fourth Quarter (source : _press.aboutamazon.com_).

**Now let’s look at who tweets ?**

If we focus on the number of likes for the tweets during the year, we get the following dataframe showing that “_fillbeforeshill_” is the top 1.
```python
AMZN2019\_rt = pd.DataFrame(AMZN2019\[\["username", "retweetCount", "likeCount"\]\])  
AMZN2019\_rt.sort\_values(by="likeCount", inplace=True, ascending=False)  
AMZN2019\_rt\[:10\]
```
![](../../img/1__HxzKcfcXB4snp__4jPXa5nQ.png)

His tweet is not really a financial tweet and that can explain why it had got so many likes.

![](../../img/1__rgXL5xoBT35QF4UOIoH4JA.jpeg)
undefined

On the side of the most followed users that tweeted about $AMZN during the year, “_fillbeforeshill”_ is not present and that makes sense because of who dominates the top 10 ranking, media.

To make this ranking, I had to use the information about the username as I said in the data presentation at the beginning.

userframes = \[userJan, userFeb, userMar, userApr, userMay, userJun, userJul, userAug, userSept, userOct, userNov, userDec\]
```python
AMZN2019\_user = pd.concat(userframes)  
AMZN2019\_user.reset\_index(inplace=True)  
AMZN2019\_user.drop(columns=\['index'\], inplace=True)  
AMZN2019\_user.sort\_values(by=\['followersCount'\], inplace=True, ascending=True)
```
![](../../img/1__p4sUkYPFmeC4QQLPk8ihRQ.png)

Reuters is the top 1 with 22 813 881 followers, the next are all media as well excepted for Jim Cramer and @OM.

For the rest of my analysis, I had to focus on English tweets only, so I looked at what languages where used in the corpus. I made a dataframe counting the tweets by the language used for the entire year.
```python
tweetsperlang = pd.DataFrame(AMZN2019\['lang'\].value\_counts())  
tweetsperlang.reset\_index(inplace=True)  
tweetsperlang.columns = \["Language", "Number"\]  
tweetsperlang.loc\[len(tweetsperlang.index)\] = \['other', tweetsperlang.iloc\[6:37, 1\].sum()\]  
tweetsperlang.drop(tweetsperlang.loc\[6:37\].index, inplace=True)
```
![](../../img/1__mSI1bG9yIqwOGikw4m__xRg.png)

We observe that English is the most used language during the entire year concerning this subject.

### **II - What’s in the tweets**

Let’s look at the tweets them self by using the famous library [**NLTK**](https://www.nltk.org/) firstly. This library is used to work with human language data. It allows text processing as classification, tokenization, stemming…  
To start, I had to clean the tweets from special characters, links, emojis etc, so I used **regular expressions** (regex) with the library _“re”_ to do that for all the corpus. I created a function that I applied on the column ‘content’ and used .
```python
import re  
import nltk  
nltk.download('stopwords')  
nltk.download('punkt')  
from nltk.corpus import stopwords  
from nltk import word\_tokenize  
stop = stopwords.words('english')

def cleaning\_tweets(text):  
   text = str(text)  
   text = text.lower()  
   text = re.sub(r'http\\S+', ' ', text)  
   text = re.sub(r'\\b\[a-zA-Z\]\\b', ' ', text)  
   text = re.sub(r'\\d+', ' ', text)  
   text = re.sub(r'\[^a-zA-Z0-9\]+', ' ', text)  
   text = re.sub(r'\\s+', ' ', text)

   text = text.split()

   return text

AMZN2019\_eng\["content\_modified"\] = AMZN2019\_eng\['content'\].apply(lambda x: cleaning\_tweets(x))

AMZN2019\_eng\["content\_modified"\] = AMZN2019\_eng\["content\_modified"\].apply(lambda x: ' '.join(\[word for word in x if word not in (stop)\]))
```
To show the output of this, let’s take a tweet and see the difference :

> Original tweet : “Meng’s Arrest Vs. The Rule Of Law $QQQ $AAPL $AMZN [https://t.co/AnCqdd8cuq](https://t.co/AnCqdd8cuq)”

> Cleaned tweet : “meng arrest vs rule law qqq aapl amzn”

Now that we have proper tweets, we are able to tokenize it with NLTK and obtain statistics from them. Word tokenization is the task of splitting sentences into words.  
I used that to **count each words** in the entire corpus of tweets and see the most common.
```python
tweets =  AMZN2019\_eng\['content\_modified'\].str.lower()  
word\_counts = Counter(word\_tokenize('\\n'.join(tweets)))

amzn\_freq = pd.DataFrame.from\_dict(word\_counts, orient='index').reset\_index()  
amzn\_freq.columns=\["Word", "Frequency"\]  
amzn\_freq.sort\_values(by=\['Frequency'\], inplace=True, ascending=True)
```
![](../../img/1__1nITZVvealM2WbNSRtIrTg.png)

As the output leads to a lot of stock index, I decided to remove the main ones and create a word cloud of the new result.
```python
from wordcloud import ImageColorGenerator

wc = WordCloud(mask=mask,max\_words=2000, color\_func=image\_colours, background\_color='white')  
wc.generate\_from\_frequencies(word\_counts)  
wc.to\_image()
```
![](../../img/1__Yhpg6QXvPuA4XaQ5nf2mIA.png)

The vocabulary of finance is clear and obvious. Important words can be spot such as **“buy”**, **“option”** or **“call”**.

I did the same process for the **description** of all the users that tweeted during the year and I had this word cloud :

![](../../img/1__pQCSXFLbxnjcOv7MQJQBfg.png)

The most common word in the Twitter’s description of all the users that tweeted about $AMZN during 2019 is the word **“Trader”**. That means that even if you can describe yourself as you want on Twitter, having this word as the most frequently is meaning that the tweets concern financial markets and not another thing. We can spot as well words such as “adviser”, “investor”, “funder”, “investment”…

### III- Sentiment Analysis and the market

The goal of my analysis was to compare on the one side, the feeling on twitter about $AMZN and on the other side the evolution of the market.  
As I introduced at the beginning, I used FinBERT to classify the tweets and get a label (positive, negative or neutral). To use the model, I worked with the [**Transformers**](https://huggingface.co/transformers/) library from HuggingFace. The library allows to use pre-trained models to do different NLP tasks such as classification, text generation… As HuggingFace says on its platform, it is backed by the two most popular deep learning libraries, [PyTorch](https://pytorch.org/) and [TensorFlow](https://www.tensorflow.org/), permitting to train your models with one then load it for inference with the other.

For my work, I used the ‘ProsusAI/finbert’ model that I presented before, available on HuggingFace.co. To set it, I worked with the AutoTokenizer and the AutoModelForSequenceClassification from the library. With these classes, the model is set up with the weights and config he has been built with.  
I applied the classifier on each month because of the size of the tables.
```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification  
from transformers import pipeline

model\_name = "ProsusAI/finbert"  
model = AutoModelForSequenceClassification.from\_pretrained(model\_name)  
tokenizer = AutoTokenizer.from\_pretrained(model\_name)  
classifier = pipeline('sentiment-analysis', model=model, tokenizer=tokenizer)

aprSentiment = (aprSentiment.assign(sentiment = lambda x: x\['content\_modified'\].apply(lambda s: classifier(s))).assign(label = lambda x: x\['sentiment'\].apply(lambda s: (s\[0\]\['label'\])),score = lambda x: x\['sentiment'\].apply(lambda s: (s\[0\]\['score'\]))))
```
Here is an example for the month of April :

![](../../img/1__ZcXzazvZDIZrXqp5Ab0w5A.png)

The classifier gives the label and the score associated to this label. The score means the level of certainty that the classifier has with the given label.

As the neutral sentiment was dominating the year and every month, I decided to only keep the Positive and Negative sentiments. It’s important to have in mind that the neutral sentiment is more frequent than the other because a lot of tweets are just ads, information about the price and more that doesn’t give any real sentiment about $AMZN.

Before looking at the sentiment each month, let’s observe the close price evolution of the stock with the library _yfinance_ (from Yahoo Finance)_._
```python
import yfinance as yf

amzn\_df = yf.download('AMZN',start='2019-01-01',  
                      end='2019-12-31',progress=False)
```
![](../../img/1__PQglNMz9MtVi1rvc2JeNFg.png)

The stock’s price increased overall, it went from $1539 in January to $1846 at the end of the year. What is interesting is watching the monthly percentage change and comparing it to the sentiment got before.

The Monthly Percentage change chart looks like this :

![](../../img/1__WEQsJu9zjBpW9BPif12FrQ.png)

And the monthly sentiment within the entire data set was :

![](../../img/1__ZKNtlCi7Z5__CNsbodFiwNg.png)

As we observe, most of the months are labelled by more positive sentiments in the tweets than negative sentiments. That can be understood by the fact the Amazon was largely positive during the entire year of 2019, increasing by 20%. The company is seen very positively from its results every quarter.

To finish my analysis, I decided to look at the sentiment from the most influencing Twitter Account from the data set. I kept 20 users from _CNBC_ to _FaceTheNation_ and observed the sentiment from their tweets. Obviously, the results lead to less sentiment data because $AMZN tweets are specific ones.
```python
top20 = AMZNsent.loc\[AMZNsent\['username'\].isin(\["CNBC", "Reuters", "ANCALERTS", "MarketWatch", "ReuterBiz", "NYSE", "jimcramer", "om", "YahooFinance", "themotleyfool", "TheStreet", "Stocktwits", "Varneyco","CBOE", "Nasdaq","CNBCnow", "ReutersIndia", "cvpayne", "ValaAshar", "FaceTheNation"\])\]

AMZNsent\_b = top20.groupby(\['month', 'label', 'month\_digit'\])\['username'\].count().reset\_index()  
AMZNsent\_b.columns = \["Month", "Label", 'Month\_digit', 'Number'\]  
AMZNsent\_b.sort\_values(by=\['Month\_digit'\], inplace=True)
```
![](../../img/1__qu3GP2c3WzXg4fg4Ism3zQ.png)

Some months have a real difference between positives and negatives. April for example, it seems that this month, the top 20 were really positive about $AMZN. The stock increased by 6,19% during this month. On the contrary, in July 2019, the top 20 was also very optimistic whereas the stock lowered by 2,88%.

Let’s look at some examples of the tweets by the top 20 during these months to understand the trend (only positive tweets).

For **April** :

> **_CNBC :_** “FANG stocks gained $600 billion since December, ‘the sky’s limit’ two, says expert (via @tradingnation) $FB $AMZN $NFLX $GOOGL”

> **_TheStreet:_** “Amazon $AMZN feels good international growth”

For **July** :

> **_Reuters:_** “@ReutersBiz Amazon’s operating expenses surged 21% second quarter invested $800 million one-day delivery program $AMZN”

> **_TheStreet:_** “The call seems reassuring investors. Shares around 0.5% after-hours. $AMZN”

### **Conclusion**

This project was a try to link the stock market with investors. It is obviously difficult to clearly see the impact of the sentiment on the market especially with the data I used because it was just on a small part of the market, $AMZN. Every investors do not talk about stocks with a twitter account, they do not talk with the $AMZN inside their tweets so it is clear that the data set I created was just to start looking at it and not really expect a quality analysis leading to great results.   
However, I was able to observe some nice things as the most frequent words in the descriptions of the users and the most frequent words in the tweet also, that was greatly reflecting financial markets and the people who tweeted during the year (“trader”, “investor”, “adviser”…). Looking at the sentiment of financial sentences with a model trained to do it was also something interesting. This led to observe a dominating positive sentiment for the entire year with some negative aspect especially from the most followed accounts during the year.

A lot of possible analysis would be interesting using Twitter or Reddit data with NLP models or other machine learning algorithms for Behavioral Finance to try to understand how the investor is taking his decisions.

The entire analysis is available on [GitHub](https://github.com/npogeant/amzn2019).

Thank you for reading this article, I hope you enjoyed !