# Predict Sparkify Churn using PySpark and AWS

Files:
- **PySpark Script (local)**: Sparkify_small_dataset.ipynb
- **AWS EMR Deployment Script**: Sparkify_full_dataset.ipynb


## Overview
Sparkify is a music streaming service in the Udacity universe. Just like other streaming services, users can choose free tier subscription with ads or paid tier without ads. They can also change their account type anytime they want. 

We will use a small dataset (128MB) to do the analisys. Then we will proceed to deployment with the Amazon Cloud Services, AWS EMR, with the full dataset (12GB). The objective is to make a model that can predict the user churn.

- Clean data: remove unnecessary data, clean NANs, create churn collum, etc..
- EDA: explore the data to see the data and the correlation with the churn column.
- Feature engineering: extract the main features  and implement standarization on the data.
- Train and measure models:  All the previous steps where conducted in a smmaler dataset. With this dataset the best model was the linearregression. 

## Aplications

- Python 3.6
- PySpark ML
- Jupyter


## Data and Feature Selection
The data has 18 columns : artist: string,auth: string, firstName,gender: string,itemInSession: long,lastName: string,length: double,
level: string, location: string,method: string ,page: string,registration: long,sessionId: long,song: string,status: long, ts: long,
userAgent: string,userId: string. It also contains  286500 records and 225 distinct users (for the small one).


7 features have been chosen for further analysis:

- gender: female = 1, male = 0
- time_from_creation: days since registration at the last user login time
- paid_user: whether the user has ever used paid tier service
- Downgrade: whether the user has ever downgraded 
- total_songs_played - total number of songs played by the user
- interactions - max user actions
- session_time -days since registration and the session



## Results

I used four models to get the baseline: Logistic Regression, Linear SVC, Decision Tree Classifier and Random Forest Classifier.
This gave us the following result with the smaller dataset:

![tests.png](images/tests.png)

With the full dataset i got this results for the F1 Score with the default parameters:

LogisticRegression:  0.7530376437311861
DecisionTreeClassifier:  0.7281994453132363
RandomForestClassifier: 0.7133317926181466
LinearSVC:  0.7569933772112831

With the Logistic and the SVC close in scoring i choose to do a cross-validation with both. The results are (f1 score):

LogisticRegression :0.7435218879141079. Best Param (regParam):  0.01 .Best Param (MaxIter):  10. Best Param (elasticNetParam):  0.0
LinearSVC: 0.755621576663529. Best Param (regParam):  0.01 . Best Param (MaxIter):  100

So the LinearSVS is slightly better, but it took a longer time to run. So for the sake of economy we will take the Logistic Regression as the final model

## Final Model (AWS)
The final model was trained on the AWS EMR and returned the above results. 

## References

Dataset provided by [Udacity](https://cn.udacity.com/).
