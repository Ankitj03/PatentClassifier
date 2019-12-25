# PatentClassifier
Classifier to predict the patent approval decision (granted vs. non-granted) for USPTO patent data.

## Problem statement and Dataset
We have a historical data of the patent applications for which we have to build a classifier to predict the approval decision. The patent application dataset consists of the id, title,summary,filing date, publication number,publication date,status and details of the authors.  

To solve with problem I have provided two solutions. First is the classification task as provided in the challenge. Second method is based on the cosine similarity of the new application with the past applications to determine if there already exists a similar patent and hence not grant an approval to the new application.

## Preprocessing
As the data provided is in the form of multiple nested json strings, the first task is to flatten the dataframe. Next we remove the unnecessary columns(authors,cities,etc.) and keep the data from only the object. 
There are 19 distinct status types which tell us about what latest action was taken on the application. We seperate these statuses into 3 categories namely:
