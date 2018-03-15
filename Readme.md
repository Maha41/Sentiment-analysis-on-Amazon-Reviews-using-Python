
<img src="https://fortunedotcom.files.wordpress.com/2017/02/amazon.gif?w=1024"></img>

--------------
# AMAZON PRODUCT AND REVIEW DATA
------------------------------------------------------------------------------------------------------------------------------  

## DATA SET DESCRIPTION:-
- This dataset contains product reviews and metadata of 'Clothing, Shoes and Jewelry' category from Amazon, including 2.5 million reviews spanning May 1996 - July 2014.
- This dataset includes reviews (ratings, text, helpfulness votes), product metadata (descriptions, category information, price, brand, and image features), and links.
- [DATA SET LINK](https://drive.google.com/open?id=0B4Hj2axlpCcxWldiajctWmY0NG8)

## FILES:-
### 'ProductSample.json'
- Consist of all the products in 'Clothing, Shoes and Jewelry' category from Amazon. Each product is a json file in 'ProductSample.json'(each row is a json file).
#### FIELDS:
- 1 Asin - ID of the product, e.g. 0000031852
- 2 Title - name of the product
- 3 Price - price in US dollars (at time of crawl)
- 4 ImUrl - url of the product image
- 5 Related - related products (also bought, also viewed, bought together, buy after viewing)
- 6 SalesRank - sales rank information
- 7 Brand - brand name
- 8 Categories - list of categories the product belongs to

### 'ReviewSample.json'
- Consist of all the reviews for the products in 'Clothing, Shoes and Jewelry' category from Amazon. Each review is a json file in 'ReviewSample.json'(each row is a json file).
#### FIELDS:
- 1 ReviewerID - ID of the reviewer, e.g. A2SUAM1J3GNN3B
- 2 Asin - ID of the product, e.g. 0000013714
- 3 Reviewer Name - name of the reviewer
- 4 Helpful - helpfulness rating of the review, e.g. 2/3
- 5 Review Text - text of the review
- 6 Overall - rating of the product
- 7 Summary - summary of the review
- 8 Unix Review Time - time of the review (unix time)
- 9 Review Time - time of the review (raw)

## ANALYSIS:-
* Analysis_1 : Sentimental Analysis on Reviews.
* Analysis_2 : Exploratory Analysis.
* Analysis_3 : 'Susan Katz' as 'Point of Interest' with maximum Reviews on Amazon.
* Analysis_4 : 'Bundle' or 'Bought-Together' based Analysis.
* Analysis_5 : Recommender System for Popular Brand 'Rubie's Costume Co'.

## DATA PROCESSING:-
#### PRODUCT DATA i.e. ProductSample.json
- Step 1: Reading a multiple json files from a single json file 'ProductSample.json' and appending it to the list such that each index of a list has a content of single json file.
- Step 2: Iterating over list and loading each index as json and getting the data from the each index and making a list of Tuples containg all the data of json files. During each iteration json file is first cleaned by converting files into proper json format files by some replacements.
- Step 3: Creating a dataframe using the list of Tuples got in the previous step.

#### REVIEW DATA i.e. ReviewSample.json
- Step 1: Reading a multiple json files from a single json file 'ReviewSample.json' and appending it to the list such that each index of a list has a content of single json file.
- Step 2: Iterating over list and loading each index as json and getting the data from the each index and making a list of Tuples containg all the data of json files.
- Step 3: Creating a dataframe using the list of Tuples got in the previous step.


## ANALYSIS 1: SENTIMENTAL ANALYSIS ON REVIEWS (1999-2014)
- Wordcloud of summary section of 'Positive' and 'Negative' Reviews on Amazon.
- VADER (Valence Aware Dictionary and Sentiment Reasoner) Sentiment analysis tool was used to calculate the sentiment of reviews.
- Sentiment distribution (positive, negative and neutral) across each product along with their names mapped with the product database 'ProductSample.json'.
- List of products with most number of positive, negative and neutral Sentiment (3 Different list).
- Percentage distribution of positive, neutral and negative in terms of sentiments.
- Sentiment distribution across the Year.
------
#### WordCloud of summary section of 'Positive' and 'Negative' Reviews on Amazon.
- Cleaning(Data Processing) was performed on 'ReviewSample.json' file and importing the data as pandas DataFrame.
- Created a function to calculate sentiments using Vader Sentiment Analyzer and Naive Bayes Analyzer.
- Vader Sentiment Analyzer was used at the final stage, since output given was much more faster and accurate.
- Only taking 1 Lakh (1,00,000) reviews into consideration for Sentiment Analysis so that jupyter notebook dosen't crash.
- Sentiment value was calculated for each review and stored in the new column 'Sentiment_Score' of DataFrame.
- Seperated negatives and positives Sentiment_Score into different dataframes for creating a 'Wordcloud'.
- Stemming function was created for stemming of different form of words which will be used by 'create_Word_Corpus()' function. PorterStemmer from nltk.stem was used for stemming.
- Function 'create_Word_Corpus()' was created to generate a Word Corpus.
###### To Generate a word corpus following steps are performed inside the function 'create_Word_Corpus(df)'
 - Step 1 :- Iterating over the 'summary' section of reviews such that we only get important content of a review.
 - Step 2 :- Converting the content into Lowercase.
 - Step 3 :- Using nltk.tokenize to get words from the content.
 - Step 4 :- Using string.punctuation to get rid of punctuations.
 - Step 5 :- Using stopwords from nltk.corpus to get rid of stopwords.
 - Step 6 :- Stemming of Words.
 - Step 7 :- Finally forming a word corpus and returning the word corpus.
- Function 'plot_cloud()' was defined to plot cloud.

 ##### WordCloud of 'Summary' section of Positive Reviews.
![positive_wordcloud](https://cloud.githubusercontent.com/assets/25044715/25310293/64218508-27af-11e7-9c7b-d8467cef5b72.png)
 ##### WordCloud of 'Summary' section of Negative Reviews.
![negative_wordcloud](https://cloud.githubusercontent.com/assets/25044715/25310299/7ba67224-27af-11e7-900b-3c0097a5c2a8.png)

#### SENTIMENT DISTRIBUTION ACROSS EACH PRODUCT ALONG WITH THEIR NAMES MAPPED FROM PRODUCT DATABASE.
- Cleaning(Data Processing) was performed on 'ProductSample.json' file and importing the data as pandas DataFrame.
- Took only those columns which were required further down the Analysis such as 'Asin' and 'Sentiment_Score'.
- Used Groupby on 'Asin' and 'Sentiment_Score' calculated the count of all the products with positive, negative and neutral sentiment Score.
- DataFrame Manipulations were performed to get desired DataFrame.
- Sorted the rows in the ascending order of 'Asin' and assigned it to another DataFrame 'x1'.
- Products Asin and Title is assigned to x2 which is a copy of DataFrame 'Product_datset'(Product database).
- Merged 2 Dataframes 'x1' and 'x2' on common column 'Asin' to map product 'Title' to respective product 'Asin' using 'inner' type.
- Took all the data such as Asin, Title, Sentiment_Score and Count into .csv file                                          
 - (path : Final/Analysis/Analysis_1/Sentiment_Distribution_Across_Product.csv)

#### LIST OF PRODUCTS WITH MOST NUMBER OF POSITIVE, NEGATIVE AND NEUTRAL SENTIMENT (3 DIFFERENT LIST).
- Segregated reviews based on their Sentiments_Score into 3 different(positive,negative and neutral) data frame,which we got earlier in step.
- Sorted the above result in descending order of count.
- Droped the unwanted column 'index'.
- Took all the data such as Asin, Title, Sentiment_Score and Count for 3 into .csv file
 - (path : '../Analysis/Analysis_1/Positive_Sentiment_Max.csv'), 
 - (path : '../Analysis/Analysis_1/Negative_Sentiment_Max.csv'), 
 - (path : '../Analysis/Analysis_1/Neutral_Sentiment_Max.csv')
 
#### PERCENTAGE DISTRIBUTION OF POSITIVE, NEUTRAL AND NEGATIVE IN TERMS OF SENTIMENTS.
- Took summation of count column to get the Total count of Reviews under Consideration.
- Percentage was calculated for positive, negative and neutral and was stored into a new column 'Percentage' of data frame.
- Taking all the data such as Sentiment_Score, Count and Percentage into .csv file
 - (path : '../Analysis/Analysis_1/Sentiment_Percentage.csv')
 
#### SENTIMENT DISTRIBUTION ACROSS THE YEAR
- Converted the data type of 'Review_Time' column in the Dataframe 'Selected_Rows' to datetime format.
- Created an Addtional column as 'Month' in Datatframe 'Selected_Rows' for Month by taking the month part of 'Review_Time' column.
- Created an Addtional column as 'Year' in Datatframe 'Selected_Rows' for Year by taking the year part of 'Review_Time' column.
- Grouped on the basis of 'Year' and 'Sentiment_Score' to get the respective count.
- Segregated rows based on their Sentiments by year.
- Got the total count including positive, negative and neutral to get the Total count of Reviews under Consideration for each year.
- Merged the dataframe with total count to individual sentiment count to get percentage.
- Calculated the Percentage to find a trend for sentiments.
- Took all the data such as Year, Sentiment_Score, Count, Total_Count and Percentage for 3 into .csv file
 - (path : '../Analysis/Analysis_1/Pos_Sentiment_Percentage_vs_Year.csv')
 - (path : '../Analysis/Analysis_1/Neg_Sentiment_Percentage_vs_Year.csv')
 - (path : '../Analysis/Analysis_1/Neu_Sentiment_Percentage_vs_Year.csv')
- Bar-Chart to know the Trend for Percentage of Positive, Negative and Neutral Review over the years based on Sentiments.
##### Bar-Chart to know the Trend for Percentage of Positive Review over the years based on Sentiments.
![positive reviews over years](https://cloud.githubusercontent.com/assets/25044715/25310310/b06b86ac-27af-11e7-8954-24dc4f330c6f.png)

##### Bar-Chart to know the Trend for Percentage of Negative Review over the years based on Sentiments.
![negative reviews over years](https://cloud.githubusercontent.com/assets/25044715/25310314/c3e740ae-27af-11e7-9523-878d8a084da4.png)

##### Bar-Chart to know the Trend for Percentage of Neutral Review over the years based on Sentiments.
![neutral reviews over years](https://cloud.githubusercontent.com/assets/25044715/25310315/d2dc46cc-27af-11e7-8b1a-3ee8cee0f224.png)

## CONCLUSION OF ANALYSIS 1:-
- From Positive WordCloud 
 - Popular words used to describe the products were love, perfect, nice, good, best, great and etc.
 - Many people who reviewed were happy with the price of the products sold on Amazon.
 - Much talked products were watch, bra, jacket, bag, costume, etc.
- From Negative WordCloud
 - Popular words used to describe the products were dissapoint, badfit, terrible, defect, return and etc.
 - Much talked products were shoes, watch, bra, batteries, etc.
- Popular product in terms of sentiments for following
 - Positive:
  - Converse Unisex Chuck Taylor Classic Colors Sneaker, Number of positive reviews:953
  - Converse Unisex Chuck Taylor All Star Hi Top Black Monochrome Sneaker, Number of positive reviews:932
  - Yaktrax Walker Traction Cleats for Snow and Ice, Number of positive reviews:676
 - Negative:
  - Yaktrax Walker Traction Cleats for Snow and Ice, Number of negative reviews:65
  - Converse Unisex Chuck Taylor Classic Colors Sneaker, Number of negative reviews:44
  - Converse Unisex Chuck Taylor All Star Hi Top Black Monochrome Sneaker, Number of negative reviews:44
 - Neutral:
  - Converse Unisex Chuck Taylor Classic Colors Sneaker, Number of neutral reviews:313
  - Yaktrax Walker Traction Cleats for Snow and Ice,Number of neutral reviews:253
  - Converse Unisex Chuck Taylor All Star Hi Top Black Monochrome Sneaker,Number of neutral reviews:247
- Sentiment Percentage Distribution.
  - Positive: 72.7 %
  - Negative: 5 %
  - Neutral: 22.3 %
  - Overall Sentiment for reviews on Amazon is on positive side as it has very less negative sentiments.
- Trend for Percentage of Review over the years
 - positive reviews percentage has been pretty consistent between 70-80 throughout the years.
 - negative reviews has been decreasing lately since last three years, may be they worked on the services and faults.

# ANALYSIS 2: EXPLORATORY ANALYSIS OF PRODUCT AND REVIEWS (1999-2014)
- Number of Reviews over the years.
- Number of Reviews by month over the years.
- Distribution of 'Overall Rating' for 2.5 million 'Clothing Shoes and Jewellery' reviews on Amazon.
- Yearly average 'Overall Ratings' over the years.
- Distribution of helpfulness on 'Clothing Shoes and Jwellery' reviews on Amazon.
- Distributution of length of reviews on Amazon.
- Distribution of product prices of 'Clothing Shoes and Jewellery' category on Amazon.
- Distribution of 'Overall Rating' of Amazon 'Clothing Shoes and Jewellery'.
- Product Price V/S Overall Rating of reviews written for products.
- Average Review Length V/S Product Price for Amazon products.
- Distribution of 'Number of Reviews' written by each of the Amazon 'Clothing Shoes and Jewellery' user.
- Distribution of 'Average Rating' written by each of the Amazon 'Clothing Shoes and Jewellery' users.
- Distribution of Helpfulness of reviews written by Amazon 'Clothing Shoes and Jewellery' users.
- Average Rating V/S Avg Helpfulness written by Amazon 'Clothing Shoes and Jewellery' user
- Helpfulness VS Average Length of reviews written by Amazon 'Clothing Shoes and Jewellery' users.
-----------------------------------
#### Number of Reviews over the years.
- Cleaning(Data Processing) was performed on 'ReviewSample.json' file and importing the data as pandas DataFrame.
- Converting the data type of 'Review_Time' column in the Dataframe 'dataset' to datetime format.
- Creating an Addtional column as 'Month' in Datatframe 'dataset' for Month by taking the month part of 'Review_Time' column.
- Creating an Addtional column as 'Year' in Datatframe 'dataset' for Year by taking the year part of 'Review_Time' column
- Grouping by year and taking the count of reviews for each year.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/Year_VS_Reviews.csv')
- Line Plot for number of reviews over the years.
![number of reviews over the years](https://cloud.githubusercontent.com/assets/25044715/25310321/04e3c8fc-27b0-11e7-84fe-0fadfab7735e.png)

#### Number of Reviews by month over the years.
- Grouping on Month and getting the count.
- Replacing digits of 'Month' column in 'Monthly' dataframe with words using 'Calendar' library.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/Month_VS_Reviews.csv')
- Line Plot for number of reviews over the years.
![number of reviews by month](https://cloud.githubusercontent.com/assets/25044715/25310322/154f423e-27b0-11e7-86ab-c33dc7748d2a.png)


#### Distribution of 'Overall Rating' for 2.5 million 'Clothing Shoes and Jewellery' reviews on Amazon.
- Grouping on 'Rating' and getting the count.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/Rating_VS_Reviews.csv')
- Bar Chart Plot for Distribution of Rating.
![distribution of rating](https://cloud.githubusercontent.com/assets/25044715/25310327/49c76ed8-27b0-11e7-9de6-df6df7fe5ff0.png)

#### Yearly average 'Overall Ratings' over the years.
- Grouping on Year and getting the mean.
- Calculating the Moving Average ith window of '3' to confirm the trend
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/Yearly_Avg_Rating.csv')
- Bar Chart Plot for Distribution of Rating.
![average overall ratings over the years](https://cloud.githubusercontent.com/assets/25044715/25310330/5c90884c-27b0-11e7-82aa-eb4345c13914.png)


#### Distribution of helpfulness on 'Clothing Shoes and Jwellery' reviews on Amazon.
- Only taking required columns and converting their data type.
- Calculating helpfulnes Percentage and replacing Nan with 0
- Creating an Interval of 10 for percentage Value.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/Helpfuness_Percentage_Distribution.csv')
- Bar Chart Plot for Distribution of Rating.
![percentage distribution of helpfulness](https://cloud.githubusercontent.com/assets/25044715/25310336/6e1cdc5a-27b0-11e7-8dfb-c92813466d22.png)

#### Distributution of length of reviews on Amazon.
- Creating a new Data frame with 'Reviewer_ID','Reviewer_Name' and 'Review_Text' columns.
- Counting the number of words using 'len(x.split())'
- Counting the number of characters 'len(x)'
- Creating an Interval of 100 for Charcters and Words Length Value.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/Character_Length_Distribution.csv')
 - (path : '../Analysis/Analysis_2/Word_Length_Distribution.csv')
- Bar Plot for distribution of Character Length of reviews on Amazon
![distribution of character length](https://cloud.githubusercontent.com/assets/25044715/25310338/7ddd52dc-27b0-11e7-8a13-63813eff2a2f.png) 

- Bar Plot for distribution of Word Length of reviews on Amazon
![distribution of word length](https://cloud.githubusercontent.com/assets/25044715/25310341/88db7a6a-27b0-11e7-9e89-38a6ee75b8fe.png) 

#### Distribution of product prices of 'Clothing Shoes and Jewellery' category on Amazon.
- Cleaning(Data Processing) was performed on 'ProductSample.json' file and importing the data as pandas DataFrame.
- Segregating the product based on price range.
 - [0-10]
 - [11-50]
 - [51-100]
 - [101-200]
 - [201-500]
 - [501-1000]
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/Price_Distribution.csv')
- Bar Chart Plot for Distribution of Price.
<img src="../Analysis/Analysis_2/Price Distribution.png">

#### Distribution of 'Overall Rating' of Amazon 'Clothing Shoes and Jewellery'.
- Grouping on Asin and getting the mean of Rating.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/Rating_Distribution.csv')
- Bar Chart Plot for Distribution of Price.
![price distribution](https://cloud.githubusercontent.com/assets/25044715/25310343/953f4bc4-27b0-11e7-94c8-643039f644ba.png)

#### Product Price V/S Overall Rating of reviews written for products.
- Merging 2 data frame 'Product_dataset' and data frame got in above analysis, on common column 'Asin'.
- Scatter plot for product price v/s overall rating
![product price vs overall rating](https://cloud.githubusercontent.com/assets/25044715/25310346/a521b1da-27b0-11e7-95cf-8a3a68a14d53.png)

#### Average Review Length V/S Product Price for Amazon products.
- Creating a new Data frame with 'Reviewer_ID','Reviewer_Name', 'Asin' and 'Review_Text' columns.
- Counting the number of words using 'len(x.split())'
- Counting the number of characters 'len(x)'
- Grouped on 'Asin' and taking the mean of Word and Character length.
- Scatter plot for product price v/s average review length.
![product price vs average review length](https://cloud.githubusercontent.com/assets/25044715/25310350/b1db10ec-27b0-11e7-9f4d-b1a200475d1e.png)

#### Distribution of 'Number of Reviews' written by each of the Amazon 'Clothing Shoes and Jewellery' user.
- Grouped on 'Reviewer_ID' and took the count.
- Sorting in the descending order of number of reviews got in previous step.
- Now grouped on Number of reviews and took the count.
- Created a interval of 10 for plot and took the sum of all the count using groupby.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/DISTRIBUTION OF NUMBER OF REVIEWS.csv')
- Scatter Plot for Distribution of Number of Reviews.
![number of reviews written by each user](https://cloud.githubusercontent.com/assets/25044715/25310353/c10483dc-27b0-11e7-9fab-aa7731fc6d5d.png)


#### Distribution of 'Average Rating' written by each of the Amazon 'Clothing Shoes and Jewellery' users.
- Grouped on 'Reviewer_ID' and took the mean of Rating.
- Scatter Plot for Distribution of Average Rating.
![average rating written by user](https://cloud.githubusercontent.com/assets/25044715/25310357/d292bd3a-27b0-11e7-9e4b-b399b33ce3d1.png)


#### Distribution of Helpfulness of reviews written by Amazon 'Clothing Shoes and Jewellery' users.
- Creating a new Dataframe with 'Reviewer_ID','helpful_UpVote' and 'Total_Votes'
- Calculate percentage using: (helpful_UpVote/Total_Votes)*100
- Grouped on 'Reviewer_ID' and took the mean of Percentage'
- Created interval of 10 for percentage
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/DISTRIBUTION OF HELPFULNESS.csv')
- Bar Chart Plot for DISTRIBUTION OF HELPFULNESS.
![distribution of helpfulness](https://cloud.githubusercontent.com/assets/25044715/25310370/e2418612-27b0-11e7-8bba-f18226e500cd.png)

#### Average Rating V/S Avg Helpfulness written by Amazon 'Clothing Shoes and Jewellery' user
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/AVERAGE RATING VS AVERAGE HELPFULNESS.csv')
- Scatter Plot.
![average rating vs avg helpfulness](https://cloud.githubusercontent.com/assets/25044715/25310373/f17fccec-27b0-11e7-88ad-e70737be61e0.png)

#### Helpfulness VS Average Length of reviews written by Amazon 'Clothing Shoes and Jewellery' users.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_2/HELPFULNESS VS AVERAGE LENGTH.csv')
- Scatter Plot.
![helpfulness vs average length](https://cloud.githubusercontent.com/assets/25044715/25310376/00f79128-27b1-11e7-8b9f-19bc90380092.png)


## CONCLUSION OF ANALYSIS 3:-
- There has been exponential growth for Amazon in terms of reviews, which also means the sales also increased exponentially. Plot for 2014 shows a drop because we only have a data uptill May and even then it is more than half for 5 months data.
- Buyers generally shop more in December and January.
- More than half of the reviews give a 4 or 5 star rating, with very few giving 1, 2 or 3 stars relatively.
- Average Rating over every year for Amazon has been above 4 and also the moving average confirms the trend.
- Majority of the reviews had perfect helpfulness scores.That would make sense; if you’re writing a review (especially a 5 star review), you’re writing with the intent to help other prospective buyers.
- Majority of reviews on Amazon has length of 100-200 characters or 0-100 words.
- Over 2/3rds of Amazon Clothing are priced between $0 and $50, which makes sense as clothes are not meant to be so expensive.
- The most expensive products have 4-star and 5-star overall ratings.
- Over 95% of the reviewers of Amazon electronics left less than 10 reviews.
- Reviewers who give a product a 4 - 5 star rating are more passionate about the product and likely to write better reviews than someone who writes a 1 - 2 star 

# ANALYSIS 3 : POINT OF INTEREST AS THE ONE WITH MAXIMUM NUMBER OF REVIEWS ON AMAZON
- Reviewer with maximum number of reviews.
- Distribution of reviews for 'Susan Katz' based on overall rating (reviewer_id : A1RRMZKOMZ2M7J).
- Distribution of reviews over the years for 'Susan Katz'.
- Percentage distribution of negative reviews for 'Susan Katz', since the count of reviews is dropping post year 2009.
- Lexical density distribution over the year for reviews written by 'Susan Katz'.
- Wordcloud of all important words used in 'Susan Katz' reviews on amazon.
- Number of distinct products reviewed by 'Susan Katz' on amazon.
- Products reviewed by 'Susan Katz'.
- Popular sub-category for 'Susan Katz'.
- Price range in which 'Susan Katz' shops.
-------------------
#### Reviewer with maximum number of reviews.
- Cleaning(Data Processing) was performed on 'ReviewSample.json' file and importing the data as pandas DataFrame.
- Grouped on 'Reviewer_ID' and getting the count of reviews.
- Sorted in Descending order of 'No_Of_Reviews'
- Took Point_of_Interest DataFrame to .csv file
 - (path : '../Analysis/Analysis_3/Most_Reviews.csv')

#### Distribution of reviews for 'Susan Katz' based on overall rating (reviewer_id : A1RRMZKOMZ2M7J).
- Only took those review which is posted by 'SUSAN KATZ'.
- Created a function 'ReviewCategory()' to give positive, negative and neutral status based on Overall Rating.
 - score >= 4 then positive
 - score <= 2 & score > 0 then negative
 - score =3 then neutral
- Calling function 'ReviewCategory()' for each row of DataFrame column 'Rating'.
- Grouped on 'Category' which we got in previous step and getting the count of reviews.
- Bar Plot for Category V/S Count.
![category vs count](https://cloud.githubusercontent.com/assets/25044715/25310381/2e08a576-27b1-11e7-8fd3-58bd9a70b666.png)

#### Distribution of reviews over the years for 'Susan Katz'.
- Grouping on 'Year' which we got in previous step and getting the count of reviews.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_3/Yearly_Count.csv')
- Bar Plot to get trend over the years for Reviews Written by 'SUSAN KATZ'
![number of reviews over years](https://cloud.githubusercontent.com/assets/25044715/25310383/3989879e-27b1-11e7-9dba-b7615ac96865.png)

#### Percentage distribution of negative reviews for 'Susan Katz', since the count of reviews is dropping post year 2009.
- Took the count of negative reviews over the years using 'Groupby'.
- Merging 2 Dataframe for mapping and then calculating the Percentage of Negative reviews for each year.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_3/Negative_Review_Percentage.csv')
- Bar Plot for Year V/S Negative Reviews Percentage
![of negative reviews over years](https://cloud.githubusercontent.com/assets/25044715/25310390/4617c7b4-27b1-11e7-8261-876ccc9d4433.png)


#### Lexical density distribution over the year for reviews written by 'Susan Katz'.
- Lexical words include:
 - verbs (e.g. run, walk, sit)
 - nouns (e.g. dog, Susan, oil)
 - adjectives (e.g. red, happy, cold)
 - adverbs (e.g. very, carefully, yesterday)
- Created a function 'LexicalDensity(text)' to calculate Lexical Density of a content.
 - Steps involved are as follows:-
  - Step 1 :- Converting the content into Lowercase.
  - Step 2 :- Using nltk.tokenize to get words from the content.
  - Step 3 :- Storing the total word count.
  - Step 4 :- Using string.punctuation to get rid of punctuations.
  - Step 5 :- Using stopwords from nltk.corpus to get rid of stopwords.
  - Step 6 :- tagging of Words and taking count of words which has tags starting from ("NN","JJ","VB","RB") which represents Nouns, Adjectives, Verbs and Adverbs respectively, will be the lexical count.
  - Step 7 :- Finally; (lexical count/total count)*100.
- Called Function 'LexicalDensity()' for each row of DataFrame.
- Grouped on 'Year' and getting the average Lexical Density of reviews.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_3/Lexical_Density.csv')
- Bar Plot for Year V/S Lexical Density
![lexical density](https://cloud.githubusercontent.com/assets/25044715/25310393/5223819c-27b1-11e7-8504-6c5a844153c2.png)

#### Wordcloud of all important words used in 'Susan Katz' reviews on amazon.
- To Generate a word corpus following steps are performed inside the function 'create_Word_Corpus(df)'
 - Step 1 :- Iterating over the 'summary' section of reviews such that we only get important content of a review.
 - Step 2 :- Converting the content into Lowercase.
 - Step 3 :- Using nltk.tokenize to get words from the content.
 - Step 4 :- Using string.punctuation to get rid of punctuations.
 - Step 5 :- Using stopwords from nltk.corpus to get rid of stopwords.
 - Step 6 :- tagging of Words using nltk and only allowing words with tag as ("NN","JJ","VB","RB").
 - Step 7 :- Finally forming a word corpus and returning the word corpus.
- Generated a WordCloud image
![important words cloud](https://cloud.githubusercontent.com/assets/25044715/25310399/6335efa6-27b1-11e7-90bc-6f91bbaab02c.png)

#### Number of distinct products reviewed by 'Susan Katz' on amazon.
- Took the unique Asin from the reviews reviewed by 'Susan Katz' and returned the length.

#### Products reviewed by 'Susan Katz'.
- Cleaning(Data Processing) was performed on 'ProductSample.json' file and importing the data as pandas DataFrame.
- Mapping 'Product_dataset' with 'POI' to get the products reviewed by 'Susan Katz'
- Took Data to .csv file
 - (path : '../Analysis/Analysis_3/Products_Reviewed.csv')
 
####  Popular sub-category for 'Susan Katz'.
- Creating list of products reviewed by 'Susan Katz'
- Created a Function 'make_flat(arr)' to make multilevel list values flat which was used to get sub-categories from multilevel list.
- Taking the sub-category of each Asin reviewed by 'Susan Katz'.
- Counting the Occurences and taking top 5 out of it.
- Took Data to .csv file
 - (path : '../Analysis/Analysis_3/Popular_Sub-Category.csv')
 
#### Price range in which 'Susan Katz' shops.
- Took min, max and mean price of all the products by using aggregation function on data frame column 'Price'.

## CONCLUSION OF ANALYSIS 3:-
- 'Susan Katz' (reviewer_id : A1RRMZKOMZ2M7J) reviewed the maximumn number of products i.e. 180.
- Susan was only 50 % of the times happy with products shopped on Amazon
<pre>

| Category | Count   |
|------|------|
|   neg  | 38|
|   neu  | 51|
|   pos  | 91|

</pre>

- Number of reviews were droping for 'Susan Katz' after 2009.
- The reason why rating for 'Susan Katz' were dropping because Susan was not happy with maximum products she shopped i.e. because the negative review count had increased for every year after 2009.
- The Average lexical density for 'Susan Katz' has always been under 40% i.e. 'Susan Katz' writting used to lack the important words.
- Most popular words used in 'Susan Katz' content were shoes, color, fit, heels, watch and etc.
- Number of distinct products reviewed by 'Susan Katz' on amazon is 180.
- Popular Category in which 'Susan Katz' were Jewelry, Novelty, Costumes & More
- 'Susan Katz' shopping Price Range
 - Minimum: 6.99
 - Maximum: 249.99
 - Average: 66.01

# ANALYSIS 4 : 'BUNDLE' OR 'BOUGHT-TOGETHER' BASED ANALYSIS.
- Check for the popular bundle (quantity in a bundle).
- Top 10 Popular brands which sells Pack of 2 and 5, as they are the popular bundles.
- Top 10 Popular Sub-Category with Pack of 2 and 5.
- Checking for number of products the brand 'Rubie's Costume Co' has listed on Amazon since it has highest number of bundle in pack 2 and 5.
- Minimum, Maximum and Average Selling Price of prodcts sold by the Brand 'Rubie's Costume Co'.
- Top 10 Highest selling product in 'Clothing' Category for Brand 'Rubie's Costume Co'.
- Top 10 most viewed product for brand 'Rubie's Costume Co'.
-----------

#### Check for the popular bundle (quantity in a bundle).
- Cleaning(Data Processing) was performed on 'ProductSample.json' file and importing the data as pandas DataFrame.
- Got numerical values for 'Number_Of_Pack' and etc from 'ProductSample.json'.
- Grouped by Number of Pack and getting their respective count.
- Took the data into .csv file
 - (path : '../Analysis/Analysis_4/Popular_Bundle.csv')
- Bar Chart was plotted for Number of Packs 
![popular bundle](https://cloud.githubusercontent.com/assets/25044715/25310412/87bd9842-27b1-11e7-9392-9aad2a648b84.png)

#### Top 10 Popular brands which sells Pack of 2 and 5, as they are the popular bundles.
- Got all the asin for Pack 2 and 5 and stored in a list 'list_Pack2_5' since they have the highest number of counts
- Got the brand name of those asin which were present in the list 'list_Pack2_5'.
- Removed the rows which does not have brand name
- Counted the occurence of brand name and giving the top 10 brands.
- Took the data into .csv file
 - (path : '../Analysis/Analysis_4/Popular_Brand.csv')
- Bar Chart was plotted for Popular brands.
![popular brand](https://cloud.githubusercontent.com/assets/25044715/25310413/91f13e04-27b1-11e7-8ca3-3bb862d1c91b.png)

#### Top 10 Popular Sub-Category with Pack of 2 and 5.
- Got all the asin for Pack 2 and 5 and stored in a list 'list_Pack2_5'
- Created a Function 'make_flat(arr)' to make multilevel list values flat which was used to get sub-categories from multilevel list.
- Got the category of those asin which was present in the list 'list_Pack2_5'.
- Counted the occurence of Sub-Category and giving the top 10 Sub-Category.

#### Checking for number of products the brand 'Rubie's Costume Co' has listed on Amazon.
- Got all the products which has brand name 'Rubie's Costume Co'
- Took the total count of the products.

#### Minimum, Maximum and Average Selling Price of prodcts sold by the Brand 'Rubie's Costume Co'.
- Took min, max and mean price of all the products by using aggregation function on data frame column 'Price'. 

#### Top 10 Highest selling product in 'Clothing' Category for Brand 'Rubie's Costume Co'.
- Took all the Asin, SalesRank and etc. whose brand is 'RUBIE'S COSTUME CO' from ProductSample.json.
- Created a Data frame for above.
- Sorted DataFrame based on sales Rank.
- Calculated Average selling price for top 10 products.
- Took the data into .csv file
 - (path : '../Analysis/Analysis_4/Popular_Product.csv'). 
- Bar Plot
![top 10 salesrank](https://cloud.githubusercontent.com/assets/25044715/25310417/9c903338-27b1-11e7-83fe-15f8c13dd26d.png)

#### Top 10 most viewed product for brand 'Rubie's Costume Co'.
- From all the Asin getting all the Asin present in 'also_viewed' section of json file.
- Counting the Occurence of Asin for brand Rubie's Costume Co.
- Creating a DataFrame with Asin and its Views.
- Getting products of brand Rubie's Costume Co.
- Merging the 2 DataFrames 'views_dataset' and 'view_prod_dataset' such that only the Rubie's Costume Co. products from 'view_prod_dataset' gets mapped. Inner type merge was performed to get only mapped product with Rubie's Costume Co.
- Sorting the DataFrame based column 'Views'
- Took the data into .csv file
 - (path : '../Analysis/Analysis_4/Most_Viewed_Product.csv')
- Took min, max and mean price of Top 10 products by using aggregation function on data frame column 'Price'
![top 10 views](https://cloud.githubusercontent.com/assets/25044715/25310420/a4c75ad6-27b1-11e7-8358-8658895c62c9.png)

## CONCLUSION OF ANALYSIS 4:-
- Pack of 2 and 5 found to be the most popular bundled product.
- 'Rubie's Costume Co' found to be the most popular brand to sell Pack of 2 and 5.
- Women, Novelty Costumes & More, Novelty, etc. are the popular sub-category in 'Clothing shoes and Jewellery' on Amazon.
- 'Rubie's Costume Co' has 2175 products listed on Amazon.
- 'Rubie's Costume Co' Selling Price Range
 - Minimum: 0.66
 - Maximum: 783.01
 - Average: 20.43
- Popular products for 'Rubie's Costume Co' were in the price range 5-15. such as 
 - DC Comics Boys Action Trio Superhero Costume Set
 - The Dark Knight Rises Batman Child Costume Kit
 - Star Wars Clone Wars Ahsoka Lightsaber, etc.
- 'Rubie's Costume Co' Selling Price Range 
 - Minimum: 4.95
 - Maximum: 12.99
 - Average: 7.24
 - Most viewed products for 'Rubie's Costume Co' were also in the price range 5-15, this confirms the popular product data.  

# ANALYSIS 5 : RECOMMENDER SYSTEM FOR BRAND RUBIE'S COSTUME CO. 
- Collaborative filtering algorithms is used to get the recomendations.
- The Recommender System will take the 'Product Name' and based on the correlation factor will give output as list of products which will be a suggestion or recommendation.
- Suppose product name 'A' act as input parameter i.e. If a user buy product 'A' so based on that it will output the product highly correlated to it.
-----------------------------------------

- Cleaning(Data Processing) was performed on 'ProductSample.json' file and importing the data as pandas DataFrame.
- Gat all the distinct product Asin of brand 'Rubie's Costume Co.' in list.
- Cleaning(Data Processing) was performed on 'ReviewSample.json' file and importing the data as pandas DataFrame.
- Created a DataFrame 'Working_dataset' which has products only from brand "RUBIE'S COSTUME CO.".
- Performed a merge of 'Working_dataset' and 'Product_dataset' to get all the required details together for building the Recommender system.
- Took only the required columns and created a pivot table with index as 'Reviewer_ID' , columns as 'Title' and values as 'Rating'.
- Created a function 'pearson(x1,x2)'. 
 - Function to find the pearson correlation between two columns or products.
 - Will produce result between -1 to 1.
 - Function will be used within the recommender function 'get_recommendations()'.
- Created a function 'get_recommendations(product_id,M,num)'. 
 - Function to recommend the product based on correlation between them.
 - Takes 3 parameters 'Product Name', 'Model' and 'Number of Recomendations'
 - Will return a list in descending order of correlation and the list size depends on the input given for Number of Recomendations.
- Created a function 'escape(t)'. 
 - Function to replace all the html escape characters to respective characters.
- Calling the recommender System by making a function call to 'get_recommendations('300 Movie Spartan Shield',Model,5)'.
 - '300 Movie Spartan Shield' is the product name pass to the function i.e. if person buys '300 Movie Spartan Shield' what else can be recommended to him/her.
 - 'Model' is passed for correlation calculation. Model is a pivot table created previously.
 - '5' is the maximum number of recommendation a function can return if there is some correlation.
 - Quantifying the correlation can be done by using correlation value given in the output.
- Taking recommendation into DataFrame for Tabular represtation.
 - Takng only those values whose correlation is greater than 0.
- Took all the recommendations into .csv file
 - (path : '../Analysis/Analysis_5/Recommendation.csv')
 
## CONCLUSION OF ANALYSIS 5:
- When '300 Movie Spartan Shield' is passed to recommender system.
- Output:  - <pre>
 
| Product Title | Correlation   |
|------|------|
|   300 Movie Spartan Deluxe Vinyl Helmet  | 0.223277|
|   Toynk Toys - 300- Spartan Sword  | 0.069275|

</pre>
    - Above given table is recomendation for '300 Movie Spartan Shield'


### Citation
- Image-based recommendations on styles and substitutes J. McAuley, C. Targett, J. Shi, A. van den Hengel SIGIR, 2015
- Inferring networks of substitutable and complementary products J. McAuley, R. Pandey, J. Leskovec Knowledge Discovery and Data Mining, 2015
