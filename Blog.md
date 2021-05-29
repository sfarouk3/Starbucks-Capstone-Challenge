
# Starbucks Capstone Challenge   

   ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/sbOverview.jfif)
              
## Table of Contents
  1.	[Project Overview](#ProjectOverview)
  2.	[Data Sets](#DataSets)
  3.	[Problem Statement](#ProblemStatement)
  4.	[Metrics](#Metrics)
  5.	[Exploratory Data Analysis and Visualization](#ExploratoryDataAnalysisandVisualization)
  6.	[Data Preprocessing](#DataPreprocessing)
  7.	[User-User Based Collaborative Filtering](#User-UserBasedCollaborativeFiltering)
  8.	[Refinement: Rank Based Recommendations](#RefinementRankBasedRecommendations)
  9.	[Evaluation and Validation](#EvaluationandValidation)
  10.	[Justification](#Justification)
  11.	[Conclusion - Reflection](#Conclusion-Reflection)
  12.	[Improvement](#Improvement)


## Project Overview <a name="ProjectOverview"></a>
This is a simulation for the customer behavior on the Starbucks rewards mobile app. Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks.

## Data Sets <a name="DataSets"></a>
The data is contained in three files:
### Portfolio file that contains offer ids and meta data about each offer
  1.	id  - offer id
  2.	offer_type - type of offer ie BOGO, discount, informational
  3.	difficulty -  minimum required spend to complete an offer
  4.	reward - reward given for completing an offer
  5.	duration  - time for offer to be open, in days
  6.	channels 
### Profile file that contains demographic data for each customer
  1.	age - customer's age
  2.	became_member_on - account creation date
  3.	gender - customers's gender
  4.	id - customer's id
  5.	income - customer's income
### Transcript file that contains records for transactions, offers received, offers viewed, and offers completed
  1.	event  - record description (ie transaction, offer received, offer viewed, etc.)
  2.	person  - customer id
  3.	time  - time in hours since start of offer. The data begins at time t=0
  4.	value  - either an offer id or transaction amount depending on the record

## Problem Statement <a name="ProblemStatement"></a>
As long as not all users receive the same offer, I need to build a recommendation system that provides the most suitable offers for each user.

## Metrics <a name="Metrics"></a>
This new recommendation system can be tested using the A/B testing concept to compare it to existing systems and evaluate the added value of this new system.

## Data Exploration and Visualization <a name="ExploratoryDataAnalysisandVisualization"></a>
in this part we'll explore the datasets to understand more the data and the relation between different variables.

### 1-Portfolio Data Set

 	10 different offers of types bogo, discount and informational offers
 	The offers' channels are concatenated in one column and needs splitting

#### All the Data Set:
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic1.jpg)
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic2.png)

### 2- Profile Data Set
 
 	1. 17000 distinct customers.
 	2. Some NaN values in the columns.
 	3. Age column has some big values – like 118, this needs to be removed.
 	4. Males have higher distribution than Females
 	5. Most of the Customers are member since 2017/2018
  
 #### Sample of the Dataset
 ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic3.png)
 
 #### Some Statistics about the numerical columns:
 ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic4.png)
 
 gender pic
 age pic
 membershpi pic
 
 ### 3-Transcript Data Set

 	1. For different event status: 'offer received', 'offer viewed', 'transaction', 'offer completed'
 	2. The transaction event has no offer id associated with it.
 	3. The Value column contains offer_ids, amount spent

 ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic5.png)
 event bar pic
 
 ## Data Preprocessing <a name="DataPreprocessing"></a>
### Cleaning Portfolio dataset
    1.  Rename the id column to 'offer_id'
    2. Encoding the channels column and offer_type column
    3. Removing the old columns (channels and offer_type)
  ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic6.png)
 ### Cleaning Profile Dataset
    1.	removing null values
    2.	removing the outliers in age column
    3.	mapping the id of the customer to a simpler form of 'user_id'
    
  #### After Removing the Null values the max age became 101 instead of 118
  ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic7.png)
  agedist pic
  
 ##### Adding a simpler mapping user id for the customers
 ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic8.png)

 ### Cleaning Transcript Dataset
    1.	extract the offer_id and the amount of money from value column
    2.	encode the 'event' column
    3.	drop the old columns 'value', 'event'
#### Data after parsing and extracting offer id   and amount columns from value column:
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic9.png)

## Implementation

## Building a Recommendation System

## User-User Based Collaborative Filtering: <a name="User-UserBasedCollaborativeFiltering"></a>
This is to be used for the Current users for Starbucks offers based on similarity and closeness to the target user.

This is done on 4 steps:
    1.	Creating user_item matrix of users as rows and offers as columns
    2.	Find the similar users to the target user sorted by similarity and on a tie it sorts by the number of used offer by each user.
    3.	Get the Offers used by the above extracted users.
    4.	Final ranking for the above extracted offers by the total number that these offers have been used and completed.


#### Sample of the similar users data frame:
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic10.png)

Sample of the top 5 recommended offers for a specific user:
['2298d6c36e964ae4a3e7e9706d1fb8c2',
 'f19421c1d4aa40978ebb69ca19b0e20d',
 'ae264e3637204a6fb9bb56bc8210ddfd',
 '9b98b8c7a33c4b65b9aebfe6a799e6d9',
 '4d5c57ea9a6940dd891ad53e9dbe8da0']

## Refinement <a name="RefinementRankBasedRecommendations"></a>

As new user will have no similarity with any of the existing users as they haven’t used any offers yet, so we can use rank based recommendation for those customers based on top offers used.

### Rank Based Recommendations: <a name="ProjectOverview"></a>
This is to be used for any new user as it ranks the offers based on the usage.
Sample of the output:
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic11.png)


## Evaluation and Validation <a name="EvaluationandValidation"></a>

As we didn’t use a machine learning model in this problem, the evaluation of this recommendation system shall be through A/B testing concept where the users are divided randomly into two paths (one for the existing recommendation system and the other for our new recommendation system).

This would show the performance of the new system over the old one, and based on the testing we can decide the way forward.


## Justification <a name="Justification"></a>

In this project we covered different types of users (existing and new) through different types of recommendation systems (Rank based and collaborative filtering)


## Conclusion - Reflection <a name="Conclusion-Reflection"></a>
From the above we can see that we can use a combination of 2 techniques for Offers Recommendations
1.	for New users that have never used the Starbucks offers, we can use Rank Based Recommendations.
2.	for Current users for Starbucks offers, we can use User-User Based Collaborative Filtering.

## Improvement <a name="Improvement"></a>
The recommendation system can be further improved by combining different types of recommendation systems like matrix factorization and other techniques; which I will consider in phase 2 of this project


  

 
 
