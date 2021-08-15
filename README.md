# PySpark_Amazon-Vine-Analysis

## Overview of Amazon Vine Paid Reviews Program Analysis
The Amazon Vine program is a service provided by SellBy. Companies like SellBy pay a small fee to Amazon in exchange for products they can provid to their Amazon Vine members. These members receive items at no/low cost and are required to publish reviews on the products they receive.

As a result of the sizable product offering and high review count provided by Amazon, data sets of reviews exceed the capabilities of Excel, SQL, and other databases. Instead, we will use Amazon Web Services to create a cloud server to house our dataset for the purpose of our analysis. The purpose of the analysis is to review the reviews data to understand if members in the Vine program are more likely to review products with a 5 star rating (on a scale of 1-5, with a 5 rating being most preferred to market Amazon products to consumers).


## Resources
Amazon Web Services (AWS)
Google Colaboratory/Colab Notebook
PySpark v 3.1.2
PostgresSQL 13
PGAdmin4 v 5.2

A series of Amazon review datasets were provided in a GZIP Compressed Tar Archive file. Data was extracted from the raw data provided and transformed into dataframes using PySpark in Google Colab before being written to an AWS Relational Database Services (RDS) server and queried in PGAdmin using PostgresSQL.

This analysis uses the review dataset for Video Games: 
https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Video_Games_v1_00.tsv.gz


## Results
#### Total Review Counts
Our original dataframe had 1,785,997 rows of distinct Amazon reviews, including both Vine program reviews (paid) and reviews created outside the Vine program (unpaid).

The original dataframe includes a customer/reviewer id number, product data, information about the review (including helpful/total votes by other customers), and a column of boolean data ('Y' or 'N') to indicate which reviews are part of the Vine program. This dataframe was distilled to just a few headings for our analysis, including: review_id, star_rating, helpful_votes, total_votes, vine, and verified_purchase. We limited our analysis to reviews with a minimum equal to or greater than 20 votes to focus on reviews most likely to be helpful. Further, we ran a simple calculation comparing helpful votes to total votes and filtered out any reviews with less than 50% of votes specified as helpful.

![Vine_Percents_DF](https://github.com/ozloty06/PySpark_Amazon-Vine-Analysis/blob/main/Vine_Percents.png)

Counts From Filtered Dataframe:
------------------------------------------------------------------------
- 102 Vine Program Reviews (paid reviews)
- 48 Vine Program 5 Star Reviews (paid reviews)
- 47% Percent of Vine Program Reviews with 5 Stars (paid reviews)
------------------------------------------------------------------------
- 64,000 non-Vine Program Reviews (unpaid reviews)
- 20,282 non-Vine Program 5 Star Reviews (unpaid reviews)
- 32% Percent of non-Vine Program Reviews with 5 Stars (unpaid reviews)
------------------------------------------------------------------------
**15% more 5 star rated paid reviews vs. unpaid reviews**

![Final_DF](https://github.com/ozloty06/PySpark_Amazon-Vine-Analysis/blob/main/Final_DF.png)


#### Additional Results
It's important to note that only 0.16% of the reviews are part of the paid Vine program. With an actual count of just over 100 reviews, this is a relatively small sample size to draw conclusions on. 

When considering verified purchases, total reviews for non-vine programs drops to a count of 15,260, of which 4,660 are verified purchases with non-Vine program reviews that had a rating of 5 stars, representing 31%.

When filtering for most helpful reviews with scores greater than 100, the Vine program had 9 out of 102, representing 8.8% of the reviews while non-Vine reviews had 3,091 out of 64,000, representing only 4.8% of the reviews. 


## Summary
Based on the limited data available for reviews created as part of the Vine (paid) program, it does appear that there may be a positivity bias for products to be reviewed with a 5 star rating. While Vine reviews represent only 0.16% of our data, this analysis shows that Vine reviews are 16% more likely to receive a 5 star rating. 

Some additional analysis was already conducted with the data available in our final dataframe, including assessing if verified purchased reviews outside of the Vine program would yield results similar to the Vine program. Our assessment shows that 5 star ratings represented 31% compared to all non-Vine reviews at 32% and Vine reviews at 47%. This indicates that verified purchase is likely not a factor for positivity bias.

On the other hand, when filtering for most helpful reviews (helpful reviews above 100), the Vine program does appear to receive a higher rating of 8.8% of reviews receiving votes for helpfulness compared to only 4.8% for non-Vine program reviews. Therefore, while a positivity bias may exist for the ratings, there may be more value in review content for consumers as they evaluate products for purchase consideration. 

Additional analysis would benefit from a larger dataset of Vine program reviews, as the small number of Vine reviews may not be a large enough sample size. However, the large difference in percentages may mean the results are still statistically significant. With about 50 Amazon review datasets available, additional Vine program data may be available and other datasets may not indicate a positivity bias or perhaps even skew for a more critical review. It may be helpful to standardize the analysis, include statistical evaluation, and broaden the study to include all of the data available.

Further, it may be helpful to evaluate all star ratings to determine if Vine reviews are skewed at all star levels with a positivity bias. An analysis of measures of central tendency could be used on star ratings for a more in-depth review of the distribution of these ratings for the Vine (paid) program and non-Vine (unpaid) reviews. 
