# PatentClassifier
Classifier to predict the patent approval decision (granted vs. non-granted) for USPTO patent data.

## Problem statement and Dataset
We have a historical data of the patent applications for which we have to build a classifier to predict the approval decision. The patent application dataset consists of the id, title,summary,filing date, publication number,publication date,status and details of the authors.  

To solve with problem I have provided two solutions. First is the classification task as provided in the challenge. Second method is based on the cosine similarity of the new application with the past applications to determine if there already exists a similar patent and hence not grant an approval to the new application.

## Preprocessing
As the data provided is in the form of multiple nested json strings, the first task is to flatten the dataframe. Next we remove the unnecessary columns(authors,cities,etc.) and keep the data from only the object. 
There are 19 distinct status types which tell us about what latest action was taken on the application. We seperate these statuses into 2 categories namely:
1. Approved (Patented Case)
2. Disapproved (All other statuses)

The Next step is to clean the title and Summary texts. The following steps are taken for text cleaning:
1. Change characters to lowercase.
2. Remove stop words like it,the,a,etc.
3. Remove punctuations and numbers.
4. Lemmatize the words ie. convert words to their root synonym.

As an added measure, we extract the 30 most common words throughout the data. Since all these words are present in most of the documents, we remove them so as to be able to find similarity between documents in a better way.

The last step is to sort the data by their Filing Date so that we can determine the precedence of patent approval.

## Classification Method
If we treat the dataset as a simple classification problem, the only features affecting the result would be the content of the summary and title. We take 5 different types of classification models best suited for binary text classification:
1. Logistic Regression
2. Multinomial Naive Bayes
3.RandomForestClassifier
4.Support Vector Classifier
5.XG Boost Classifier

To choose the best classification model, we use the score metrices: accuracy,precison and recall. Based on all three, Support Vector Machine gives the best scores. Hence, we'll use Support Vector Classifier as our model. 
However, in the classification report we see that the model predicts 1 in all cases on the test set. The score is very misleading.

We can see that the model is penalized for predicting the majority class in all cases. The scores show that the model that looked good according to the accuracy is in fact barely skillful when considered using using precision and recall that focus on the positive class. This is possible because the model predicts probabilities and is uncertain about some cases. These get exposed through the different thresholds evaluated in the construction of the curve, flipping some class 0 to class 1, offering some precision but very low recall.

## Cosine Similarity Method
After carefully examining the dataset and understanding the problem statement, we can see that the problem should not be treated as a classification problem. Since, this data is about the patents filed and approved/disapproved, we should look out for plagarism in the data. If a patent filed recently is similar to a patent filed before and has been already approved, the recently filed patent should be rejected. 

After examining the ROC curve and the classification report, we can say that the model has slightly better performance than the classification models.

## APIs
The system supports two APIs:
1. Accept a new patent and return a prediction. (predict_patent)
2. Accept and record the true label for a patent. (record_patent)

In addition, to decide when to update the model, everytime we record the true label of the patent, we predict the label and check if the model predicts it correctly. After 5 failed attempts at predicting the right label, we call the update_model fuction that retrains the model with the new recorded data.

To check, the APIs, we use Postman to send new data to the system and get back a prediction:
![Alt text](https://github.com/Ankitj03/PatentClassifier/blob/master/Screen%20Shot%202019-12-26%20at%207.16.00%20PM.png)
