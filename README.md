# Best_Headphones

## Table of Contents
* [Project Overview](#Project-Overview)
* [Resources](#Resources)
* [Background](#Background)
* [Procedure](#Procedure)
* [Findings](#Findings)
* [Future Work](#Future-Work)

## Project Overview

1. Acquired headphone data using the Best Buy API (~9000 entries and ~200 features). 
2. Cleaned and processed the data for a variety of downstream applications
3. Performed an exploratory analysis where I uncovered:
   * Features such as cost, rating and savings that are correlated with one another
   * Best headphone brands based on customer reviews
   * Recurring terms that appear in the best reviewed headphone descriptions. These terms embody the qualities that make these headphones the best on the market.
  
   ![alt text](https://github.com/MSlobody/Best_Headphones/blob/main/Data/EDA_Overview_Figure.jpg)
   
## Resources
Python Version: 3.9.7\
Packages: numpy, pandas, scipy, seaborn, matplotlib, urllib3, wordcloud, Pillow. See requirements.txt.\
Best Buy API: https://developer.bestbuy.com/  \
API Documentation: https://bestbuyapis.github.io/api-documentation/?shell#getting-started  \
Word Cloud Adapted From:  https://github.com/amueller/word_cloud  
   
## Background
Whether on a long commute, listening to a podcast with the loud background noises completely removed, or running at the gym energized by the deep bass and crisp vocals of your favourite song, headphones enrich our daily lives in various ways. 

Question: What manufacturers have the best reviewed headphones on the market? Are there any recurring features that the best headphones embody (such as noise cancellation)? Are the best reviewed headphones also the most expensive? 

In this project I am interested in exploring a headphone dataset I acquired using the Best Buy API to answer these questions. 

## Procedure

To begin, I developed a function to extract data from Best Buy using their API, which is applicable to any product category. The function 'query_bestbuy()' in "Acquiring_Data.ipynb' requires only two inputs, the name of a product category and an API key. Optionally the user can provide the manufacturer name, the number of pages they would like to obtain amongst a variety of other parameters to customize their search. 

#### Example: Obtaining specific information about the 5 most reviewed Sony headphones.
```
df_SonyHeadphones = query_bestbuy(category = "Headphones",api_key = '#############',
                                   manufacturer = "Sony",sort = "customerReviewCount.dsc",page_size = 5,
                                   show = "name,salePrice,customerReviewCount,customerReviewAverage,longDescription")

#Obtaining information about Sony Headphones on page 1. 
#There a total of 84 pages left.

df_SonyHeadphones

```
![alt text](https://github.com/MSlobody/Best_Headphones/blob/main/Data/SonyHeadphones.PNG)

By performing a general search for all headphones, I acquired 8884 products with 189 attributes.
Next, this dataset was cleaned by removing attributes(columns) and products(rows) that contain mostly missing data.
Columns with numeric data (such as weight and height) were then reformatted from a string to a float for subsequent analysis.
Some headphone manufacturer names contained special characters such as "®" or "™", which were removed to ensure consistent labelling.
Lastly, products that were incorrectly labelled as headphones were removed. 

In summary the cleaned and processed dataframe was now ready for analysis, despite having some missing data:

![alt text](https://github.com/MSlobody/Best_Headphones/blob/main/Data/MissingData_FilteredHeadphones_dataframe.PNG)

## Findings

#### With a filtered dataset of 7742 headphones, I first explored the summary statistics for price, average rating and number of reviews. 
- The most expensive and least expensive headphones are both in fact earbuds. The "Shure - KSE1500 Electrostatic Earphones System - Black" costs a whopping $2999.98 USD! These are the first ever single-driver electrostatic sound isolating earphone that provides an unparalleled high-quality audio listening experience to its customers. To contrast this, the cheapest earbuds are from the company Rocketfish, which are only $0.99. On average, a headphone costs aproximately $102.

- The median reviewed headphone has an average rating of 4.3, which is likely inflated by the large number of products with a single 5 star review. Nearly 60000 people have reviewed the "Apple airpods with charging case 2nd generation in white", which is significantly more than the median review count of 25. 

#### Next, I examined how the various numeric features correlate with one another using a spearman correlation:

![alt text](https://github.com/MSlobody/Best_Headphones/blob/main/Data/Correlation_Heatmap.png)

A few patterns emerged. We expect large items to be heavy, which we see as a cluster of positive correlation around the weight, depth, width and height features. These large items also tend to be more expensive, with a positive correlation between both weight and price and size and price. Shipping is usually free for expensive items and companies tend to increase the cost of shipping for cheaper products. This explains the strong negative correlation between shipping cost and product price. Lastly, there is a weak positive correlation between price and average customer reviews, which indicates that more expensive items are weakly associated with better reviews. 

#### Which brand is the best reviewed? 

![alt text](https://github.com/MSlobody/Best_Headphones/blob/main/Data/Best_Reviewed_Headphones_Boxplot.png)

To answer this, I filtered for manufacturers that have at least 5 distinct headphone models and a median number of reviews of 5 across their products. As a result, the top 11 brands are shown here ranked by their median averaged headphone rating. Some well-established brands such as Apple, Bose and Beats by Dr. Dre are known to provide a wide variety of headphone options that are beloved by their customers and generally dominate the market. The Australian headphone brand Audiofly tops this list, and is unfortunately out of business as of 2021. Shokz designs the best open-ear headphones tailored for edurance athletes. Belkin is known for their affordable wireless earbuds.

#### What are some recurring terms that appear in the best reviewed headphone descriptions:

![alt text](https://github.com/MSlobody/Best_Headphones/blob/main/Data/Headphones_Wordcloud.PNG)

This reflects the qualities of the best headphones. A wireless, comfortable, noise cancelling headphone that performs well and is convenient to transport.

## Future Work

In the future I would like to extend this project to develop a recommender system. A user would provide an input, such as cost, headphone type, rating amongst a variety of other optional parameters. With this data, a k-nearest neighbors algorithm will identify the best suited product(s) from the filtered dataset of 7742 headphones.
