---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: How to Ride the Shopping Cart
permalink: /
---

## Introduction
When you look back at the last decade of your life, what do you see? Which are the most imporant things that pop into your mind? Family vacation? Graduating from university? First kiss? Winning a competition? That moment when you, for the first time, held your baby in your arms? Or is it maybe some sad event, like loosing a loved one? While these situations significantly shape our lives and make us who we are today, life of each individual consists of other, "background" processes and occasions. They almost certainly don't come to our minds as being momentous, but they for sure affect our mood, relationships, and, overall, build our everyday life. One of these things is - you won't believe - grocery shopping! It follows, at first sight, that grocery shopping is such an irrelevant part of our life. After all, it is nothing more than a routine, right? Doing laundry, brushing your teeth, reading generic e-mails from some random EPFL associations you've never heard of... - it seems like this is all done in a self-acting manner, automatically and mechanically. However, if you take a closer look, grocery shopping imposes heavy demands on our time, and has a direct impact on what we eat, how we spend our money and manage our time, which consequently influences our social life and well-being as a whole.

Let’s face it: grocery shopping is not a skill most of us would intentionally invest effort in pursuing. Nevertheless, people quite often find themselves in an unpleasant situation. Picture this! You want to prepare amazing spicy devil eggs for the movie night with your friends, but you're missing chilli peppers and apple cider vinegar. Next thing, you find yourself in the super market looking for what you went for in the first place, but shiny packages and beautiful colors are occupying your attention and your senses. Suddenly, you return home with two bags of groceries, and guess what! - you didn't buy jalapeños and vinegar! Why is it like that? Well, not having sometimes a clear idea about our inventory state, we end up being tempted into all sorts of treats that are over and above our necessities. Or, we plan to showcase our cooking abilities by preparing a plentiful lunch with several courses. So, we go shopping for various and numerous products - many of which we don't need or are not needed in that quantity. Guided by our instinct and instant desires when grocery shopping, rather than being prepared and aware of what we really need, we end up with the fridge full of everything, often surprised by the number of items which have to be thrown away because expiration date passed two months ago! 


![alt text1][interstellar]

[interstellar]: Figures/interstellar.jpeg "Title Text"

Through this story, we aim to address the following research question: What is the interplay between income and expenses? In particular, we are interested in the following: How do households choose to organize their limited annual income according to their shopping expenses? Can we infer different household types based on the relation between their income and transaction statistics? Are there some demographic properties of the household's indicators of this relation as well? To begin answering our questions, we first perform data exploration on the Dunnhumby dataset. 
#### This dataset contains the following information: 
1. Demographic information, including annual income, family size, homeownership (...);
2. Product information, including the manufacturer, product category (soft drinks, fruits, vegetables, ...), and other information;
3. Information about transactions, including the purchased products, their prices, time of purchase (...).


#### Amongst others, we expect the following factors to be decisive in how peole spend their annual income:
1. Household income
    - In our dataset we have several categories ranging from below 15,000 to over 250,000 USD p.a.
2. Shopping habits
    - It mainly depends on the day of the week
2. Advertisement campaigns
    - Some households take active participation in campaigns which significantly lowers the expenses
3. Products vary in price
    - Some products are much more expensive than others; nonetheless, their purchase is unavoidable
4. Household's preferences
    - Households with lower income tend to buy low-budget goods, as opposed to households with higher income which seek to purchase high-end goods (like organic food)
5. Demographics
6. Number and age of children in the family



# **1. Are you sure you're earning enough to buy that fancy organic avocado?**
## *You know, not everyone is in the top 1%...[^1]*
[^1]: 99% of all people are not in the top 1%, to be precise :)

![alt text1][avocado]

[avocado]: Figures/avocado.jpg "Title Text"

As expected, our analysis demonstrated that one of the major factors in detecting the household expenses is the annual income of a given household. The main challenge in utilizing this information from the datasets was the fact that we didn't have exact nummeric data about the household incomes. Instead, we were provided only with different income categories. On the bright side, there is a meaningful order in the categories as they are intervals. The most challenging thing here was finding an appropiate nummerical representation for each income category. We did so by fitting the data to the lognormal distribution. Even before seeing the income distribution, we already assumed that it would be log-normal, as this is the usual distribution of positiv random variables. This follows from the central limit theorem, applied to variables which cannot be negative (like household income). 

Namely, we could simply plot the number of households contained within every income category, and get the following plot:

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_20.html"></iframe>
{% endraw %}

Whilst there is nothing wrong with this plot on the first sight, in-close analysis reveils a large problem with it: the bins are not equidistant. That is, in the second lowest bin (15-24K) there is only a range of 9,000 USD, while there is 49,000 USD income range in the second highest bin (200-249 USD). Accordingly, it is biased towards bins of a larger range. For a fair plot, we need to divide all the bins by their range. By doing that, we get an approximate *income distribution density* of the number of households per 1,000 USD income range. Since the data is non-nummeric, we still won't be able to get a nice and continuous distribution of the income, but at least it will be unbiased towards larger bins. 

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_25.html"></iframe>
{% endraw %}

By just looking at the above-given plot, one may already get the feeling that it looks like the log-normal distribution. In order to actually verify this, we need to use the maximum likelihood estimation to infer the parameters of the log-normal distribution. Given that the income categories are still non-numerical values, we must convert them into numbers. Naively, we were tempted to simply take the average value of the lower and the upper bound of each bin. However, bearing in mind that the data approximately follows a log-normal distribution, we have to use the geometric mean of the upper and lower bound (easy to prove) [^x].

[^x]: The proof is left to the reader

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_26.html"></iframe>
{% endraw %}

# **2. Don't throw away your bills!**
## *Statistics is your friend...*

#### Average bill amount
As our ultimate goal in this research is to be able to measure the spending habits of different households, we should, for that reason, compute the average amount of each household's bills. Every visit of a particular household to a store is uniquely identified in the dataset. We can do this in two steps:

1. We sum the total bill amounts for each household. 
2. We average across all visits which a given houshold made over the whole time period.

Here you can see the distribution of the average bill amount. On average, people spend around 30 USD per visit to a local supermarket. However, there are several outliers. This is furtherly investigated later on in the analysis. 

[//]: # ( {% raw %} <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_16.html"></iframe> {% endraw %})

[//]: # ({% raw %} <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_18.html"></iframe> {% endraw %} )

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_29.html"></iframe>
{% endraw %}
Concurrently, we are also interested in finding the average number of products purchased per household. Why is this important? The relevance of it is reflected through bills of a large families. This household category will always have higher bills per visit to the supermarket, simply because they have to buy food for more family members. At the same time, smaller and wealthier families (perhaps without childern), are expected to purchase fewer products, often for higher price. On average, people buy around 10 products per supermarket visit. 

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_31.html"></iframe>
{% endraw %}
While the average number of purchased products is a really good indicator of a households' spending habits, we argue that the average bill amount is actually dependent on two very different parameters. It is to be expected that the average number of products is proportional to the household size (after all, rich and poor families drink and eat rougly the same amounts of food[^y]). Simultaneously, the average price per purchased product is expected to be more closely related to the wealth of a given household (for example, wealthy families might be incited to purchase more expensive wine, or buy organic food). The total bill amount is roughly equal to the number of purchased products and to the price of the average product. Therefore, small wealthy families could have similar total bill amounts as poor large families. In this respect, we are interested in the distribution of the average median product price. We use the meadian for calulating the average, as it is more robust to outliers.  

[^y]: Assuming nobody is starving due to the poverty in the United States

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_33.html"></iframe>
{% endraw %}

One of the first things we learned in elementary school is a precious rule applicable in a daily life: the average is equal to the sum divided by the quantity. When it comes to grocery shopping, this is no different. The average bill we pay in supermarkets will be dependent on how many items we purchase and what is their average price. Obviously, there exists a 1:1 correspondence between the number of purchased items and the total bill we have to pay at the checkout. As the variation in price of everyday items is not large, the variance is very small:

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_35.html"></iframe>
{% endraw %}

Things become less obvious when we compare the average median product price with the average bill amount. The main factor which causes uncertainty here is the number of products, which varies greatly. It depends on how often we go shopping and how many family members we need to feed with a one "go" to the local supermarket. Thereby, we have a very significant variance in the following plot:
{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_34.html"></iframe>
{% endraw %}




### Analysis of the Joint Distribution 
Given our insight into how much money, on average, households spend during each visit to a supermarket and how the representative product price varies, we are interested to find out if the household income plays a crucial role in the spending habits. Our expectation is that it does, and that households with lower income spend less and buy cheaper (for example non-organic) products as opposed to households with higher income.

#### Household Income vs. Spending habits
Using a boxplot we can view the distribution of the statistic across each household income category and perform a comparison very efficiently.

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_37.html"></iframe>
{% endraw %}

It is surprizing to see, that  extreme spending were mostly observed for medium and lower income households. After having a closer look at the data, we found out that this is mainly due to the fact that there are significantly more househlolds in the average-income range, as in the high-income range[^k]. Therefore, we also expect more outliers in the average-income range.

[^k]: That's why we call them the "top 1%" and not the "top 40%"

We also created the same plot for the average median product price:

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_41.html"></iframe>
{% endraw %}
This is relevant, as we expect wealthier households not necessarily to buy larger quanitites of products (which is mostly responsible for the average bill amount), but rather to purchase individual more expensive items. 

# **3. Don't forget to exploit the weekend discounts!**

#### Coupon usage

Our analysis showed that campaigns play a major role in how people spend their money. 
In deriving the true amount of money that households spend on every visit to their local supermarket, we took into account the coupon value. 
Households which regularly participate in loyality programs, may end up paying less money, even if they purchase more expensive products. 
Let us find out what the total amount of coupons used is and how it varies with annual income!

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_44.html"></iframe>
{% endraw %}

The reason why coupon values are negative is, becasue they decrease the final bill. On this bar plot, we see a somewhat unexpected trend. 
People with medium to upper medium income use more coupons as opposed to the people with lower income. 
Interesting sight is that people with high income are also heavy users of coupons. 
While it is difficult to draw any certain conclusions, we can speculate that people with high income are more careful with what they spend money on. 
After all, that might be the reason why they have such high income. 
At the same time, household with lower income might not be able to profit so much from discounts, as high-reward loyality programs are more benefitial to people who actually spend larger amounts of money. 

#### Daily Expenses

Interesting information can be found by looking into the day of the week when purchases have been made. 
While we don't know the exact date (and thereby day of the week) from when the data was recorded, it is enough to group the transactions by the *day number* module *7*, in order to plot the transactions throughout an average week. 

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_46.html"></iframe>
{% endraw %}

While we cannot be certain about which day this weeks starts with, it is to be assumed that that the two days with a larger amount of transactions are either Friday and Saturday or Saturday and Sunday. If the data is from the US, it is likely that most purchases have been made on Sunday, as people tend to have more time for shopping, and the stores are usually open on Sunday. We contacted the company which provides this dataset for more information about such details, and are currently waiting for a response. 

# **4. Buy only what you need, or at least what you think you need.**
## *The types of products you buy can say a lot about you...*

#### Most Purchased Product

We want to find out on which products the different households spent most of their income in, and how this depends on their income categories. After analyzing the data, we found that the households from every category spent most of their money on gasoline. Let us see the distribution in USD for every income category:

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_53.html"></iframe>
{% endraw %}

The distribution is highly concentrated around the middle to upper middle income categories. The reason is that households with lower income can't afford owning a vehicle, however the reason for the upper income is somewhat unclear. It could be that they use other means of transportation required by their job.

Setting gasoline aside, we are also interest in how the expenses are distributed in the other product categories. For this, we will use a sankey plot because it is an excellent way to visualize flow of information between two subsets. From it, we will have a clear idea about how the expenses from income groups map to product categories. We will use 200 most purchased products ordered by their quantity.  We can easily visualize how the quantity of the purchased products is distributed and later search for patterns in the expenditure.

TODO: add the Sanky plot


TODO: the Engles curves. Didn't we say we wanna use the ones from the main.ipynb notebook, as the polynomial fits weren't exactly great?
{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_60.html"></iframe>
{% endraw %}

# **5. Know who you are, but more importantly who you are not.**

### Demographic Analysis of Household Groups

In order to understand how family values influence the balance between household's expenses and income, we will analyze the demographic properties across 4 groups of households:

1. Households with low income and low expenses (41%)
2. Households with low income and high expenses (31%)
3. Households with high income and low expenses (9%)
4. Households with high income and high expenses (19%)

We generated these groups using the average income and average expenses, by splitting the households into: below (or above) average income and below (or above) average expenses.

From the age distribution of the household groups, we can make a few interesting observations. Among the households with the youngest members there is not a lot of variety in the income-expenses balance. Among the households with members of younger working ages high expenses seem to dominate. As we move to the households with older members lower expenses are more prevalent.

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_65.html"></iframe>
{% endraw %}


#### Marital Status (watch out...it's a trap!!!)

The datasets we used also contains information about the marital status of the individual households.  They are divided into: 
1. `A` or *Married*
2. `B` or *Single*
3. `U` or *Divorced*

The following plot very effectively illustrates the usual differences between "single" and "married" life. We observed that households with married members have more often lower income and must balance with lower expenses, while the opposite is true for single member households, as they more frequently have higher income and are able to indulge in higher expenses. 

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_66.html"></iframe>
{% endraw %}

#### Homeowner Type

Another interesting future included in the dataset is, whether families own or rent the property they live in. 
It is understandable that households, which are able to afford their own place of residence, entail probability of a higher income and expenses, while renters usually have better sense of utilizing their limited income.

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_67.html"></iframe>
{% endraw %}

#### Household Composition

When considering the different household compositions indicated in the data, we do not observe a great variety of the results. 
This feature doesn't seemt o be a good discriminator. 
However, we do observe two interesting discrepancies which can also be expected: couples with no children have the highest chance to be in the group with the highest income and expenses, while single parents have a harder time paying their bills.

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_68.html"></iframe>
{% endraw %}

#### Household Size
The size of a given household also displays very little effect on the spending habits. 
However, we can observe a trend which demonstrates us that the probability of having lower income and expenses decreases as the household size increases. 
This is so because only households with higher income can afford to have more children. 
Nevertheless, this circumstance makes their expenses increase as well.

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_69.html"></iframe>
{% endraw %}

#### The tradeoff between having children and having money
We can infer the demographic records for the number of children from other features such as household size and household composition. 
In this context, we observe in the plot a similar trend as in the previous analysis of a household size.
Having children costs money!

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_70.html"></iframe>
{% endraw %}

# **The big picture**
*TODO: Add ML analyses*


