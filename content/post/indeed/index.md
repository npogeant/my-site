---
title: How to break into Finance — Indeed & Python
description: >-
  Looking for a job can become difficult with the amount of offers referenced on
  job listing websites. As a young graduated, I understood…
date: '2021-03-08T16:34:30.467Z'
categories: []
keywords: []
slug: /@npogeant/how-to-break-into-finance-indeed-python-5cca7637e114
---

Looking for a job can become difficult with the amount of offers referenced on job listing websites. As a young graduated, I understood that when I was searching for an internship. It became challenging once I saw all the different job titles, sometimes with long sentences, with terms I didn’t even know or even worse, without any specifications about what the job was. In fields like finance where new job labels are created everyday, you can quickly get lost and lose motivation.

Therefore, I decided to look into Finance job offers to get some results and eventually get some key elements for someone that would want to work in the field.

_Let’s start with the data…_

### **Indeed job offers**

![](../../img/1____2i3clEZ3daF5oP5VqF2Fg.jpeg)

I searched for strong data with interesting attributes on job offers. I considered Indeed as the perfect job platform, it is the more used one. I firstly tried to scrape Indeed pages with the keyword “finance”. The dataset collected was good but not enough and with limits, most of the rows were duplicated. Therefore, I decided to look for a tool with a better logic and robustness than my script. I chose the web data extractor software, [**Octoparse**](https://www.octoparse.com). Octoparse allows creating extraction of data with a live window of the website you scrape. You build your extracting circuit with multiple options. For my case, the extractor is great especially because it allows creating a pagination which is perfect for a website like Indeed. With some time on Octoparse, you would be able to get data from a vast array of sites.

To have the most offers possible (having in mind that most jobs postings stay active for 30 days), I chose to extract data from **_indeed.com_**, **_uk.indeed.com_** and **_ca.indeed.com_** with the keyword “**_finance”_** .  
Octoparse automatically extracts some of the data from pages, such as the title of each offers, the URL, the company name, the company rating, the summary of the post and the date. I wanted one more information and so, I added the extraction of the salary. This data is less informed than the other information but from what I got, it was enough.

The dataset looks like this :

![](../../img/1__hZn2OtdMZmuk2syVZufWZA.png)

I cleaned the dataset before working on it. As you can see, some rows have NaN values but I considered keeping NaN values for the Salary column because it is an information that I will work on later and it is not a issue for the analysis.

After removing nul values I got 3 homogeneous datasets.

I merged the 3 datasets into one and the shape of the main table was **2183 rows (jobs offers)**. The offers went from “just posted” to “30+ days” which is a good representation of the financial job market of January/February 2021 as I did the extraction in late February.

### Main Results

![](../../img/0__tJXeWeVgYZ70Gs8f.jpg)

The analysis I did has the aim to output some ideas from the financial job market and mainly Indeed job offers referring to finance in 3 countries : The United States of America, Canada and The United Kingdom.

I went through every single information I got, from the location of the offers to the words used in the summary. I did a basic statistic analysis with clear visualizations and also some Natural Language Processing work to try to understand better what the employers want.

_Let’s now look at these results…_

#### Where to work in Finance ?

To maximize your chances of finding a job, looking into the top cities or the top companies in a field is something important. Obviously, the more you have offers, the more you will have opportunities to be hired. However, the more you have offers, the more you will have competitors against you, so you would have to be better than them to stand out.

As the location specified on Indeed can be a city or an entire address, I had to clean the column with the next function (duplicated for every city with multiple addresses) :
```python
jobs.loc\[jobs.Location.str.contains('New York'), 'Location'\] = 'New York'
```
Once that done, I grouped the data by location to get the top 10 cities by job offers and it looked like that.

![](../../img/1__eKOTrFP07725jHowHW9GXg.png)

London is the top 1 with 229 job offers, followed by two Canadian cities, Toronto and Montreal. New York is at the bottom of the top 3 with 87 offers.

Now that we have the places to work in finance, let’s take a look at which companies are the most offering.

I started by looking at the number of companies in the dataset.
```python
jobs.Company.nunique()
```
I counted 1410 unique companies from Morgan Stanley to East West College. There are all types of companies in the dataset.

The ranking after a groupby function is the following :

![](../../img/1__IX5lrpGCBAPFA8nZEJLUvA.png)

We observe that the Royal Bank of Canada is leading the top and that the Bank of Montreal is following. That can be surprising but the two are part of the 5 biggest banks of Canada and are within the 100 largest banks in the world. Facebook is the third company offering job in Finance with 19 job offers. We can spot Deloitte, the famous Anglo-American services network, but also Citigroup, JPMorgan Chase Bank and finally KPMG another network from the Big Four.

However, looking for a job is not just choosing the position but also the company. Therefore, it is important to know the company you are applying for. Indeed has a feature of rating allowing applicants to have an idea about the company. I looked at these ratings from 1 to 5 (on 5).
```python
jobs\_rating = jobs\[\['Company', 'Company\_Rating'\]\]  
jobs\_rating.columns = \['Company', 'Rating'\]  
jobs\_rating.sort\_values(by='Rating', inplace=True, ascending=False)  
jobs\_rating.drop\_duplicates(inplace=True)
```
![](../../img/1__7EhMoOl5Y0bHe121f0MFnQ.png)

One of the best rated companies is Linley & Simpson, a real estate enterprise in the UK. We can observe two companies rated 1, AgencyAnalytics and Toyota Subaru Sheboygan.

It can be interesting to look at the ratings of the companies offering the most in Finance.
```python
jobs\_rating.loc\[jobs\_rating\['Company'\].isin(topjobs\_bycompany\['Company'\].tolist())\]
```
![](../../img/1__ATD__iPqjsi8aLp8S8QScpg.png)

Facebook is the best company from the employees, RBC/KPMG/Deloitte are the second ones. We notice that overall these companies are well rated.

Now that we know which companies and cities to choose in the financial world, we need to know the title of the position that we have to target on.

#### In which jobs ?

Job positions in finance are various, therefore I decided to gather titles into main ones to be able to analyze the distribution of main titles in the field. From 1705 titles before cleaning to 18 titles after, I was able to differentiate Financial Analysts to Financial Managers, Directors, Assistants… Then, I created a bar chart counting the number of offers by job titles.

Before cleaning :

![](../../img/1__iayyqfiUVegqAEHmhPly8w.png)

After cleaning :

![](../../img/1__gIVCJR9BSxj2IuTYH0Ij4A.png)

We observe Financial Analysts outperforming the others with 717 job offers, followed by Financial Managers and Financial Directors.

If we now take a look at the locations where are these different jobs are working, the results are alluring.

![](../../img/1__auyOsBUBKj9lTkaKyVmyWg.png)
![](../../img/1__rSxNh2I6WXXerZF9VGPXIg.png)
![](../../img/1__xkV3QWtSM4gnq1ujUUzf6A.png)

**Financial Analysts** are mostly based in London with almost half of them being there, then Toronto and Montreal with 40% cumulative. New York is their fourth most represented city. with just 10%.

**Financial Managers** are also mostly based in London but with almost the same numbers in Toronto. We can spot Menlo Park being in the top 3 which is interesting to note.

**Financial Directors** are differently distributed with Toronto being the first location and London the second one.

We know more about job titles but what about **types of employment** in the dataset ?   
To get the information, I associated every titles with ‘“Intern” as Internship, titles with ‘“Apprentice” as Apprenticeship and the others as Full Time jobs.  
I got the following results :

_Full Time :_ **1926 offers** _Intern :_ **235 offers** _Apprentice_ : **22 offers**

Now that we have an idea of the different positions in finance and their distribution, let’s try to see which one is the **highest paid** by analyzing the salaries.

To do that, I needed to work on the salaries column because the information in Indeed is stands as “XXX a year”. I had to clean this up and convert it to float numbers.  
I used some regular expressions and created some function to get to the results. I needed also to convert £ into $, so I divided the dataset to deal with £ and I used a library to convert it correctly.

Let’s see the distribution of salaries :
```python
salaryperyrs.Salary.describe()
```
The highest salary equals to **$350,000** and the least one is **$10,051**. The median salary is **$44,212** which is quite good I would say.

When we now look at the most paid jobs, we get these results :

Risk Manager       $111,000                   
Financial Director $102,632                   
Trader             $90,000

At the contrary, the least paid jobs are _Accountant_ and _Financial Assistant_ with less than $28,000 on average.

The highest positions are in these companies :

Xero                                                   $350,000                   
Los Angeles County Department of Human Resources       $325,563                 Medical University of South Carolina                   $251,489

Companies with the lowest wages are mostly in College, such as _Cambridge Regional College_ or _MK College_.

Finally, which cities pay the most ?

Los Angeles       $325,563                   
Charleston        $251,489                   
Denver            $237,500

And the least are cities such as _Morecambe_ or _Barry_ in the UK.

To finish this analysis, I looked at the summary of the offers and work with NLP processing to get some outputs.

#### **How to get in Finance ?**

To analyze the summary of the offers, I decided to gather summaries by titles first and then to look at the entire dataset.

First of all, I cleaned up the summaries with regular expressions (clean\_sum is a special function with regex inside):
```python
sum\_clean = pd.DataFrame(offer\_sum.Summary.apply(lambda x :clean\_sum(x)))  
sum\_clean.Summary = sum\_clean.Summary.apply(lambda x: ' '.join(\[word for word in x if word not in (stop)\]))
```
Once cleaned, let’s tokenize and lemmatize with the NLTK library :
```python
lemmer = WordNetLemmatizer()  
sum\_clean.Summary = sum\_clean.Summary.apply(lambda x: word\_tokenize(x))  
sum\_clean.Summary = sum\_clean.Summary.apply(lambda x : \[lemmer.lemmatize(y) for y in x\])
```
The summary now tokenized and lemmatized, we are able to show the frequency of each word in the dataset (by titles firstly). I used the _Counter()_ function to create a frequency dataframe and built 3 wordclouds for Financial Analysts, Traders and Financial Assistants.

**Financial Analyst :**

![](../../img/1__c6M__MgNxyF91lWeSNFIYLg.png)

We can spot the lexicon of analysis, business in its summary. Management looks important for a Financial Analyst, working as a team too, being a support for the company... Overall, the summary is mostly based on the financial lexicon. In the middle, we observe the word “bachelor” meaning that having a bachelor degree should be essential.

**Trader :**

![](../../img/1__wsE9Fk7jJtc0uXORiC82SQ.png)

We see another lexicon than the previous wordcloud. It is clearly focused on the market, trading, assets… The Trader needs to have “strong skills”, “abilities” about financial markets.

**Financial Assistant :**

![](../../img/1__Eg6aVf1Ds__LLMMvAJsxXuA.png)

A financial assistant has to be a “support”, he has to assist, to be part of a “team”, he operates, reports and is responsible.

Let’s conclude with the frequency in the **entire corpus of summaries** and try to notice if some interesting words stand out.  
The ranking looks like this :

![](../../img/1__HYMtaxru8oZ__AhSTc__47nQ.png)

What I find out compelling is the large distribution difference between _experience_ and _degree_ in the corpus of financial job offers. We observe that experience is twice as present as degree. Being experienced is clearly more important than having a degree. It doesn’t mean that having a degree isn’t important but you will need to gain financial experiences during your classes to face other applicants.

### Conclusion

This analysis had the aim to output results from a job offers with the keyword “**finance**”. It can be reproduced with any other keyword and it allows to understand what types of data we can find on Indeed.   
It could be a good way to use Indeed data for **personal motivations**, finding a job, targeting the perfect cities of companies. Moreover, the frequency of words in the summaries can be a tool to create better resume for example.

Thank you for taking the time to read the article, I hope you enjoyed it !  
You can find the code on my [**GitHub**](https://github.com/npogeant/indeed-finance) if you are interested.