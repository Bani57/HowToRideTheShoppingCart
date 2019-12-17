---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: How to Ride the Shopping Cart
permalink: /
---

## Introduction
Let’s face it: grocery shopping is not a skill most of us intentionally invested effort in pursuing. However, people more than often find themselves in situations where they return home with two bags of groceries and realize that they are missing the only item they initially were out for. Sometimes, not having a clear idea about our inventory state, we end up being tempted into the all sorts of treats that are over and above our necessities. Or we buy products to showcase our cooking abilities for the family dinner but somehow, we end up with the fridge full of everything, hoping to roll up our sleeves tomorrow. We are often surprised by the number of items we throw away because the expiration date passed two months ago! 

In this story we try to address the following research question: What is the interplay between income and expenses? In particular, we are interested in the following: How do households choose to organize their limited annual income according to their shopping expenses? Can we infer different household types based on the relation between their income and transaction statistics? Are some demographic properties of the household's indicators of this relation as well? To begin answering our questions, we first perform data exploration on the Dunnhumby dataset. 
#### This dataset contains the following information: 
1. Demographic information, including annual income, family size, homeownership, ...
2. Product information, including the manufactur, product category (soft drinks, fruits, vegetables, ...) and other information
3. Information about transactions, including the products bought, their prices, time of purchase, ....


#### There are several factors that contribute to how people spend their annual income:

1. Income category
    - In our dataset we have several categories ranging from below 15,000 to over 250,000 USD p.a.
2. Shopping behavior
    - It mainly depends on the day of the week
2. Campaigns play a major role
    - Some households take active participation in campaigns which significantly lowers the expenses
3. Products vary in price
    - Some products are much more expensive than others, nonetheless their purchase is unavoidable
4. Households preferences
    - Households with lower income tend to buy low-budget goods, as opposed to households with higher income that buy high-end goods (like organic food)
5. Demographics



# **1. Are you sure you're earning enough a year to be buying that?**
## *You know, not everyone is in the top 1%...[^1]*
[^1]: 99% to be precise :)

We noted that one of the major factors in determining the household expenses is the household income category. The main challange in utilizing this information from the datasets was the fact that we didn't have exact nummeric data about the household incomes. Instead, we were provided only with different income categories. Looking on the bright side, there is meaningful ordering of the categories as they are intervals. The main challenge here was finding an appropiare nummerical representation for each income category. We did so by fitting the data to the lognormal distribution. Even before seeing the income distirbution, we already assumeed that it would be log-normal, as this is the usual distribution of positiv random variables. This follows from the central limit theorem, applied to variables which cannot be negative (like household income). 

Naively, we could simply plot the number of household in every income category and get the following plot:

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_20.html"></iframe>
{% endraw %}

While there is nothing wrong with this plot on first sight, careful analysis reveils a large problem with it: The bins are not equidistant. That is, in the second lowest bin (15-24K), there is only a range of 9,000 USD, while in the scond highest bin (200-249 USD), there is 49,000 USD income range. Therefore, it is biased towards bins of larger range. For a fair plot, we need to divide all the bins by their range. That way, we get an approximate *income distribution density* of the number of households per 1,000 USD income range. As the data is non-nummeric, we will still not be able to get a nice and continues distribution of the income, but at least it will be unbiased towards larger bins. 

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_25.html"></iframe>
{% endraw %}

By just looking at this plot, we already get the feeling that it looks like the log-normal distribution. In order to actually verify this, we need to use maximum likelihood estimation to infer the parameters of the lognormal distribution. As the income categories are still non-numerical values, we also need to convert them into numbers. Naively, we were tempted to simple take the average between the lower and the upper bound of every bin. However, given that the data approximately follows a lognormal distribution, we have to use the geometric mean of the upper and lower bound (easy to prove) [^x].

[^x]: The proof is left to the reader

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_26.html"></iframe>
{% endraw %}

# **2. Don't throw away the bills!**
## *Statistics is your friend...*

#### Average bill amount
To measure the spending habits of the households, we compute the average amount of each households' bills. Every household visit to a store is uniquely identified. We do this in two steps:

1. We sum the total bill amounts for each household. 
2. We average across all visits a given houshold made over the whole time period.

Here you can see the distribution of the average bill amount. On average, people spend around 30 USD per visit to a local supermarket. However, there are several outliers and this is futher investigated later in the analysis. 

[//]: # ( {% raw %} <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_16.html"></iframe> {% endraw %})

[//]: # ({% raw %} <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_18.html"></iframe> {% endraw %} )

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_29.html"></iframe>
{% endraw %}
At the same time, we are also interested in finding the average number of products purchased per household. This is relevant as large families will alsways have higher bills per visit to the supermarket, simple because they have to buy food for mor family members. At the same time, smaller and wealthier families (perhaps without childern), are expected to purchase fewer products but often for higher prices. On average, people buy around 10 producs per supermarket visit. 

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_31.html"></iframe>
{% endraw %}

While the average number of producs purchased is a very good indicatior of a households' spending habits, we argue that the avarege bill amount is actualy dependend ot two very different parameters. It is to be expected that the average numebr of products is proportional to the household size (after all, rich and poor families drink and eat rougly the same amounts of food [^y]). At the same time, the average price per product purchased, is expected to be closer related to the wealth of a given household (for example, wealthy families might be inclined to purchase more expensive wine, or buy organic food). The total bill amount, is rougly equal to the numbe of producs purchased, and the price of the average product. Therefore small wealthy families could have similar total bill amounts as poor large families.  Therefore, we are interested in the distribution of the average median product price. We use the meadian for calulating the average, as it is more robust to outliers.  

[^y]: Assuming nobody is starving due to poverty in the United States

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_33.html"></iframe>
{% endraw %}

TODO

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_34.html"></iframe>
{% endraw %}

TODO

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_35.html"></iframe>
{% endraw %}

### Analysis of the Joint Distribution 

Now that we have insight on how much money, on average, people spend on each visit to the store and how does the representative product price bought vary, we are interested to see does the household income play a crucial role in this. Our expectation is that it does, and that households with lower income spend less and buy cheaper products as opposed to households with higher income.

#### Household Income vs. Average Bill Amount
Using a boxplot we can view the distribution of the statistic across each household income category and perform a comparison very efficiently.


{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_37.html"></iframe>
{% endraw %}

To our surprise, extreme spending are mostly observed for medium and lower income households. This be just due to the fact that there are significantly more househlolds in the average-income range, so we also expect more outliers. At the same time, in the highest-income categories, there are just a few households, so it is unlikely to find any extreme outliers. 

We also created the same plot for the average median product price:

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_41.html"></iframe>
{% endraw %}


# **3. Don't forget to exploit those weekend discounts!**
## *Is this even necessary to mention...*

*TODO: Add analyses on coupon usage and spending based on day of the week*

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_44.html"></iframe>
{% endraw %}

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_46.html"></iframe>
{% endraw %}

# **4. Buy only what you need, or at least what you think you need.**
## *The types of products you buy can say a lot about you...*

*TODO: Add analyses on income/expenses depending on product categories*

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_53.html"></iframe>
{% endraw %}

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_60.html"></iframe>
{% endraw %}

# **5. Know who you are, but more importantly who you are not.**
## *Always stick to your family values...*

*TODO: Add analyses on income/expenses depending on demographics*

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_65.html"></iframe>
{% endraw %}

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_66.html"></iframe>
{% endraw %}

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_67.html"></iframe>
{% endraw %}

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_68.html"></iframe>
{% endraw %}

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_69.html"></iframe>
{% endraw %}

{% raw %}
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" src="./iframe_figures/figure_70.html"></iframe>
{% endraw %}

# **The big picture**
*TODO: Add ML analyses*
