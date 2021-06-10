---
title: "What the Crypto Market looks like\_? — An interactive d3.js Bubble Chart"
description: >-
  Data visualization is the way to communicate results of data analysis. It
  means creating stories, visual context to make data easier to…
date: '2021-06-09T19:34:02.920Z'
categories: []
keywords: []
slug: >-
  /@npogeant/what-the-crypto-market-looks-like-an-interactive-d3-js-bubble-chart-14a64cd4089e
---

![](img/0__YUYMuLCXaDistny__.jpg)

**Data visualization** is the way to communicate results of data analysis. It means creating stories, visual context to make data easier to understand. It has become always more important with all the large datasets we face today.

As a data analyst greatly interested into data visualization and every tool of representing findings, I decided to dive into **d3.js**, the famous JavaScript library. What I like the most about it is the fact that it allows you to create what you want, exactly the way you want it. You are not limited by the library, which can be the case with other visualization tools.

In this article, I will explain how I did the chart and show some of the results about the crypto market.

The visualization I created is the following one (available on [Observable](https://observablehq.com/d/4af37b5b43cce051)) :

<iframe width="100%" height="721" frameborder="0"
  src="https://observablehq.com/embed/@npogeant/crypto-market-top-100-currencies?cells=viewof+variable%2Cchart"></iframe>

### Cryptocurrency Market Bubble Chart

**June 04 2021**

I made this bubble chart with HTML, CSS and JavaScript, using the library d3.js. I exported my code on the largest interactive data visualization website, [Observable](https://observablehq.com). The site allows you to use notebooks to create dataviz.

I wanted to analyze the cryptocurrency market not because I am a huge fan of crypto but because it is a interesting market, with great variables to put in a chart. Thus, I used the [CoinMarketCap](https://coinmarketcap.com) API to get my data. Basically, the dataset is constituted of the top 100 crypto on the website on the day of June 6 2021. CoinMarketCap ranks them by their market capitalization. The more your market capitalization is, the higher you will be in the ranking.

Let’s pass to the code…

#### Creating a bubble chart

I will not explain everything I did to do it, the code is on the notebook on Observable and my github, but I will highlight the main aspects of it.

Firstly, I had to process the data, this is one example of what I was obliged to modify :
```javascript
market\_cap: market\_cap.replace(/\\,/g, '').replace(/\\./g, '').slice(0,-2)
```
Indeed, the dots and commas where a problem, so I removed them in almost every column.

Once the data connected, and because it was a csv, I had to create a hierarchy of it so that each node gets parameters allowing the building of the chart.
```javascript
const root = d3.hierarchy({children: data})  
 .sum(function(d) { return d\['market\_cap'\]; })  
 .sort(function(a, b) { return b\['market\_cap'\] - a\['market\_cap'\]; })
```
In the example above, I took the market cap variable to have my hierarchy.

Then, I created the svg and all the circles associated to the nodes (every currency). I entered, appended groups (g) and used the ‘transform’ attribute to place each circle. The size of the circles are equal to the ‘r’ variable of each row (which are created within the hierarchy).
```javascript
const node = svg.selectAll(".node")  
 .data(pack(root).leaves())  
 .enter().append("g")  
   .attr("class", "node")  
   .attr("transform", function(d) { return "translate(" + d.x + "," + d.y + ")"; })

  
node.append("circle")  
       .data(pack(root).leaves())  
      .attr("id", function(d) { return d.id; })  
      .attr("r", function(d) { return d.r; })
```
Finally, I added the labels by appending texts. The first one are the symbol variable and the second one are the variable used to create the circle, I decided to have market capitalization as the initial variable of the chart.
```javascript
node.append("text")  
        .attr("class","labels")  
        .attr("dy", ".2em")  
        .text(function(d) {  
            return d.data.symbol ;  
        })  
        .attr("font-family", "Roboto")  
        .attr("font-size", function(d){  
            return d.r/5;  
        })  
        .attr("fill", "white")  
        .style("text-anchor", "middle")  
    
  node.append("text")  
        .attr("class","SecondLabels")  
        .attr("dy", "1.8em")  
        .text(function(d) {  
            return d.data.market\_capClean;  
        })  
        .attr("font-family", "Roboto")  
        .attr("font-size", function(d){  
            return d.r/7;  
        })  
        .attr("fill", "white")  
        .style("text-anchor", "middle");
```
I also added a tooltip with mouseover, mousemove and mouseleave as function.

Finally, I wanted it to be interactive, allowing the choice of the variable in the bubble. Thus, I created a update function with one parameter and selection/transition on the different element of the svg.
```javascript
svg.node().drawData = function updateData(variable) {

....

}
```
Details of the function are in the notebook, the variable updating the chart’s data is obtained by the 4 radio buttons above the svg.

Let’s look at the chart itself, what is interesting to notice…

#### Some results

The crypto market is special, we spot directly the predominance of Bitcoin when the bubbles are composed of market capitalizations. The second currency by capitalization is Ethereum with half the BTC value.

Results change from the trade volume, the US Dollar is leading the top which is normal because the dollar is the most stable currency. The difference between Bitcoin and Ethereum volume is much smaller than between capitalizations.

From the price bubbles, we notice different things. The first one is the fact that some coins are based on Bitcoin and that explains why Bitcoin is not the first currency by price. The top 1 is YFI (yearn.finance), a token created by iEarn, a financial tool that spot yield fluctuations and allow more liquidity.

The last results are from the circulating supply of the coin. We can spot directly that one coin is greatly surpassing others : Shiba Inu (SHIB). This coin is almost the same as Dogecoin, it is a token based only on the image of a Shiba Inu. It is a meme coin, with a really low price and a lot of enthusiasm which explained the massive supply.

To conclude, creating this chart was really enjoyable, I learned a lot and I saw how d3 was structured.

I hope that this was interesting and that you learned many things !