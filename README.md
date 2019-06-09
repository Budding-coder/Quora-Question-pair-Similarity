# Quora-Question-pair-Similarity

## Description

Quora is a place to gain and share knowledge—about anything. It’s a platform to ask questions and connect with people who contribute unique insights and quality answers. 
This empowers people to learn from each other and to better understand the world.

Over 100 million people visit Quora every month, so it's no surprise that many people ask similarly worded questions. 
Multiple questions with the same intent can cause seekers to spend more time finding the best answer to their question,
and make writers feel they need to answer multiple versions of the same question. Quora values canonical questions because they provide a better experience to
active seekers and writers, and offer more value to both of these groups in the long term.

## Problem Statement

1. Identify which questions asked on Quora are duplicates (or slightly differently worded) of questions that have already been asked. 
This improves the customer experiance .
2. This could be useful to merge the questions and instantly provide answers to questions that have already been answered.
3. We are tasked with predicting whether a pair of questions are duplicates or not.

## Sources/Useful Links

Source : https://www.kaggle.com/c/quora-question-pairs , we can get dataset and problem from this link.

Useful Links
1. Discussions : https://www.kaggle.com/anokas/data-analysis-xgboost-starter-0-35460-lb/comments
2. Kaggle Winning Solution and other approaches: https://www.dropbox.com/sh/93968nfnrzh8bp5/AACZdtsApc1QSTQc7X0H3QZ5a?dl=0
3. Blog 1 : https://engineering.quora.com/Semantic-Question-Matching-with-Deep-Learning
4. Blog 2 : https://towardsdatascience.com/identifying-duplicate-questions-on-quora-top-12-on-kaggle-4c1cf93f1c30

## Real world/Business Objectives and Constraints

1. The cost of a mis-classification can be very high. So we have to minimize it.
2. You would want a probability of a pair of questions to be duplicates so that you can choose any threshold of choice.
3. Here we should get a probability that, given Que 1 is similar to Que 2 and probability lies between 0 and 1. If the probabaility is greater than choosen threshold probability, then merge the answers for those questions.
4. No strict latency concerns.
5. Interpretability is partially important to understand why questions are merged

## Quora question pair productionization:-

    Given a pair of questions we determine whether they are duplicates or not .

In productionizations,

1. There will be millions of questions and if we compare the question with these million questions, 
then we have to evaluate a million test points which are pairs of questions and it is very time taking.
2. So to solve this problem, given a new question, instead of comparing it with all the million questions, 
we can decrease the set of questions we should compare it with.
3. We can use simple fast searching scheme tool, we can break up millions of questions in database into list of 
most important words through similar word compitition through inverted indices.
4. Google does this process of inverted indixes . It searhces similar questions using algorithms called inverted indixes. 
Inverted indexes are most used in text search systems in the world and it is very very fast.
5. inverted indixes method is depends on key words. So we try to find the particular key word in all the questions in database, 
for a particular given question and for matched words we give more importance .
6. There are variations of inverted indices like distributed inverted indixes etc., depending on dataset size.
So with process, instead of comparing the question with all the questions in database, 
now we have subset of questions which is smaller and we compare the given question with this subset.
7. This method gradually reduces the question comparision from million comparisions to just few comparisions, 
by just comparing keywords.
8. This is a optimization hack. There is no low latency requirments

## Data Overview

1. Data will be in a file Train.csv
2. Train.csv contains 5 columns : qid1, qid2, question1, question2, is_duplicate
. is_dupliate is Yi here which belongs to {0,1}, and using qid1 ,qid2, question1, question2 we can construct Xi. We have to find whether the question is duplicate or not. 3. If is_duplicate = 0, that means the questions are not duplicates. If is_duplicate = 1, then questions are duplicates.
4. Size of Train.csv - 60MB
5. Number of rows in Train.csv = 404,290

Class 0 = questions are not duplicates

Class 1 = questions are duplicates

## Type of Machine Leaning Problem

1. It is a binary classification problem, for a given pair of questions we need to predict if they are duplicate or not.
2. We have to featurize the data to determine whether the questions are duplicate or not. This is binary classification problem.

## Performance Metric

Source: https://www.kaggle.com/c/quora-question-pairs#evaluation

### Metric(s):

1. log-loss : https://www.kaggle.com/wiki/LogarithmicLoss . It is the metric for this problem. Here though it is a binary classification problem, 
we dont want output to be just 0 or 1.
2. We want probability value which lies between 0 to 1 , to determine whether the question is duplicate or not. 
So when we take probability value, then log loss is one of the best metric.
3. Log-loss is the primary KPI - Key Performance Indicator
4. Binary Confusion Matrix . This is secondary performance metric, where we can compute precision , recall, TPR, TNR, FPR, FNR

## Train and Test Construction

1. We build train and test by randomly splitting in the ratio of 70:30 or 80:20 whatever we choose as we have sufficient points to work with.
2. The model is built based on present day questions. If we give any new question for this model, then it determines whether this question is duplicate of already answered question.
3. Type of Questions asked change over time. And model also should change over time. So if we had a time stamp for each of question pairs then we can break the dataset based on time axis and sorted based on time and take the oldest 70% data as train data and newest 30% as test data. And this best way to split due to data change over time.
4. But here dont have time stamp and so we can't do temporal splitting.
5. So we can just randomly split train and test data.






