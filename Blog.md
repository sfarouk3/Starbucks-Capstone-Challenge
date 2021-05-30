
# Starbucks Capstone Challenge   

   ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/sbOverview.jfif)
              
## Table of Contents
  1.	[Project Overview](#ProjectOverview)
  2.	[Data Sets](#DataSets)
  3.	[Problem Statement](#ProblemStatement)
  4.	[Metrics](#Metrics)
  5.	[Exploratory Data Analysis and Visualization](#ExploratoryDataAnalysisandVisualization)
  6.	[Data Preprocessing](#DataPreprocessing)
  7.	[Implementation: Recommendation System](#User-UserBasedCollaborativeFiltering)
  8.	[Recommendation System Refinement](#RefinementRankBasedRecommendations)
  9.	[Implementation: Model](#ModelImplementation)
  10.	[Model Refinement](#RefinementModel)
  11.	[Evaluation and Validation](#EvaluationandValidation)
  12.	[Justification](#Justification)
  13.	[Conclusion - Reflection](#Conclusion-Reflection)
  14.	[Conclusion - Improvement](#Conclusion-Improvement)
  


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
As long as not all users receive the same offer, I need to build a recommendation system that provides the most suitable offers for each user along with a machine learning prediction model to predict the effectiveness of Starbucks offers.
## Metrics <a name="Metrics"></a>
The traing and testing accuracy will be the used metrics to evaluate the machine learning prediction model  
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
 Gender Distribution  
 ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/SBgender.png)  
 Age Distribution  
 ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/SBage.png)  
 Membership Year Distribution  
 ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/SBPie.png)  
### 3-Transcript Data Set
 	1. For different event status: 'offer received', 'offer viewed', 'transaction', 'offer completed'
 	2. The transaction event has no offer id associated with it.
 	3. The Value column contains offer_ids, amount spent
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic5.png)  
Offers Status  
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/SBeventbar.png)
 
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
  ![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/sbage2.png)
  
##### Adding a simpler mapping user id for the customers
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic8.png)

 ### Cleaning Transcript Dataset
    1.	extract the offer_id and the amount of money from value column
    2.	encode the 'event' column
    3.	drop the old columns 'value', 'event'
#### Data after parsing and extracting offer id   and amount columns from value column:
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic9.png)

## Implementation: Recommendation System <a name="User-UserBasedCollaborativeFiltering"></a>

### User-User Based Collaborative Filtering:
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

## Recommendation System Refinement <a name="RefinementRankBasedRecommendations"></a>

As new user will have no similarity with any of the existing users as they haven’t used any offers yet, so we can use rank based recommendation for those customers based on top offers used.

### Rank Based Recommendations: <a name="ProjectOverview"></a>
This is to be used for any new user as it ranks the offers based on the usage.
Sample of the output:
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/pic11.png)

##  Implementation: Model <a name="ModelImplementation"></a>
Now I'll build a supervised Machine Learning Model on the valid offers data set to predict whether an offer will be completed or not.
I will use three models and compare their results:

DecisionTreeClassifier  
GradientBoostingClassifier  
Random forest model  

## Model Refinement <a name="RefinementModel"></a>
Using the GridSearchCv, I've tuned the number of RandomForst  estimators to reach the best estimator, and the accuracy did improved from 85.6% to 87% by setting the number of estimators to 150.  
 New RandomForestClassifier evaluation after tuning:  
Training accuracy:1.0000  
Test accuracy:0.8718 
RandomForestClassifier feature importance:  
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/rf_refined.png)

## Evaluation and Validation <a name="EvaluationandValidation"></a>
Below is a summary of the results using different models:
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/modelsummary.png)  

and below the details with feature importance:
1. DecisionTreeClassifier:   
Training accuracy:1.0000  
Test accuracy:0.8178  
DecisionTreeClassifier feature importance:  
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/SB_DTree.png)  

2. GradientBoostingClassifier:   
Training accuracy:0.8621  
Test accuracy:0.8598  
GradientBoostingClassifier feature importance:  
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/SB_GB.png)  

3. RandomForestClassifier:   
Training accuracy:0.9909  
Test accuracy:0.8513  
RandomForestClassifier feature importance:  
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/SB_RF.png)  

4. RandomForestClassifier - After refinement (Using GridSearch):   
Training accuracy:1.0  
Test accuracy:0.8718  
RandomForestClassifier feature importance:  
![intro_image](https://github.com/sfarouk3/Starbucks-Capstone-Challenge/blob/main/Starbucks%20pics/rf_refined.png)  

## Justification <a name="Justification"></a>

In this project we covered different types of recommendation systems (Rank based and collaborative filtering) and different supervised machine learning models to predict the effectiveness of Starbucks offers.


## Conclusion - Reflection <a name="Conclusion-Reflection"></a>
As we can see in all the above work

1. the best clf is randome forest
2. the membership date is one of the most important features.
3. the Testing accuracy is 87% after refinement, and in can be more improved if more features were added to the customers profile like customer segmentation based on monthly money spent in Starbucks, Fan flag,..etc


## Conclusion - Improvement <a name="Conclusion-Improvement"></a>

The model can be further improved by:
1. Dataset: add more features to the customers profile like customer segmentation based on monthly money spent in Starbucks, Fan flag,..etc
2. Model: try other classifiers and more hyper parameter tuning using gridsearch.




  

 
 
