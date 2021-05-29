
# Starbucks Capstone Challenge

## Table of Contents
  1.	[Project Overview] (#Project Overview)
  2.	[Data Sets](#Data Sets)
  3.	[Problem Statement](#Problem Statement)
  4.	[Metrics](#Metrics)
  5.	[Exploratory Data Analysis and Visualization](#Exploratory Data Analysis and Visualization)
  6.	[Data Preprocessing](#Data Preprocessing)
  7.	[User-User Based Collaborative Filtering](#User-User Based Collaborative Filtering)
  8.	[Refinement: Rank Based Recommendations](#Refinement: Rank Based Recommendations)
  9.	[Conclusion](#Conclusion)

## Project Overview
This is a simulation for the customer behavior on the Starbucks rewards mobile app. Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks.

## Data Sets
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

## Problem Statement / Metrics
As long as not all users receive the same offer, I need to build a recommendation system that provides the most suitable offers for each user.
This new recommendation system can be tested using the A/B testing concept to compare it to existing systems and evaluate the added value of this new system.

## Data Exploration and Visualization
in this part we'll explore the datasets to understand more the data and the relation between different variables.

