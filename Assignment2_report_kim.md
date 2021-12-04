### Mini-project 2  REPORT
Date: 4.12.2021  
Kim Yukyeong
This report is available here in [my github repo](https://github.com/saugkim/abo_data2021/edit/main/Assignment2_report_kim.md), in case markdown document viewer is not installed in local machine

## Introduction

The data was collected from a fully online nine-week-long course on machine learning, hosted on the online learning management system Moodle. The goal of this project is to use two different approaches, random forest classifier and freely chosen classifier to predict students’ final grade in an online course. 

My choice for second classifier is Decision Tree classifier, and there is no scientific reason for this.



*what is the problem you are solving?*

Define problem:
   - Classification: multiclass, single label classification problem
   - Supervised learning: predict student final grades
   - Classifiers 
         mandatory: ensemble, Random Forest classifier
         my choice: Decision Tree classifier        



## Data collection
*what is the data you are collecting and what is your method of choice?*

Data is given from the course, we are using collected data of nine-week-long online course.  
We do not have to collect data but we still need to clean-up and prepare data for training model.  


**Define data** 
   - raw data as csv format: datasets_dataset.csv
   - total number of records: 107 enrolled students  
   - total number of columns: 48 (including index)   
         1 index(ID) + 1 label(Grade) + 9 grades + 36 logs + 1 Week8_Total (sum of individual grades points)
   - total number of classes: 5 (categorical labels => 0, 2, 3, 4, 5)   
   - grades related activities are 3 quiz + 3 mini-project + 3 project review

<br>

**Data Preprocessing**
    
1. tools used: pandas dataframe  

   
2. null value check and remove garbage records

    - there is no missing or null value in data set, but there are good number of 0.0 values which can be considered as null value.   
    - keep every single record with Week8_total value is not zero (records of no grade related activity are removed).
    - total 48 students ended up with grade 0.  
    - 35 records has 0.0 value in total points (Week8_Total), and these are removed completely from dataset for further analysis.  
    - finally 72 records remained.
    
<br>

3. features selection
    - dataset has total 45 features (9 grades related activities and 36 log activities)  
    - Week8_Total is not feature so that column should be removed.
    - label is last column of our dataset (it is not feature)
    - logs activities (status0, status1, status2, status3) are irrelevant data to grades (I understood from data description)
    - all logs activity of status0, status1, status2 (27) are removed from dataset.
    - keep status3 (forum activity), because there is a chance to earn few points from forum activity(at least now, no idea about past course) and I want to see effect of irrelevant data.
    - finally selected features are 9 grades related activities (quiz + project + review) and additionally 1 forum activity(Stat3, summation of 9 weeks status3 values)  
 
    Selected features for case 1: `Week2_Quiz1 Week3_MP1 Week3_PR1 Week5_MP2 Week5_PR2 Week7_MP3 Week7_PR3 Week4_Quiz2 Week6_Quiz3`

    Selected features for case 2. `Stat3 Week2_Quiz1 Week3_MP1 Week3_PR1 Week5_MP2 Week5_PR2 Week7_MP3 Week7_PR3 Week4_Quiz2 Week6_Quiz3`  
    
    
<br>
   
4. normalization of features

    because the range of values of raw data varies, I did perform standardization transforms the feature data to have zero mean and a variance of 1. More significant number might play a more decisive role while training the model. Although decision trees and ensemble methods do not require feature scaling to be performed as they are not sensitive to the the variance in the data. (ref: https://towardsdatascience.com/do-decision-trees-need-feature-scaling-97809eaa60c6)  
    
    I would say this step is not necessary or mandatory, I have not compared both cases. My result is based on the scaled feature data (zero mean unit variance) not raw value. 
    Figure shows data distributions before and after standardization transforms
    ![scaled.PNG](attachment:scaled.PNG)  
    
<br>

5. dataset splitting to training sample and validation(testing) sample

    - training samples : testing samples = 0.80 : 0.2 = 57 : 15    
    - we have 72 records total and I left about 20% samples (15 records) for validation, 57 samples are used for training classifier. Samples are splitted using sklearn train_test_split method in a stratified fashion (set random_state=42). 
    - Our samples shows imbalance in the distribution of the target classes (no records for grade 1, only 5 samples for grade 2, label 4 is most 24 records). Models seems struggle to predict label 2 sample (which samples taken to train and which is taken to testing gives different predicting ability). 
    - based on simple runs, with testing ratio >=0.30, model could not perform well. 
    
<br>
    
6. dataset is ready for training model and for evaluation 


## Data analysis 
*how did you visualise the data and how do you interpret the data visualisation?*  
- explained belows confusion matrix and important features part

<br>

*How accurate your model predicts the students’ final grade and how do these model compare against each other?*

    with 9 grades features dataset and base setting of classifier.
  - random forest classifier predicted label correctly 13 from 15 test samples.
  - decision tree classifier made predictions correctly 14 from 15 test samlples.    
  - depending on model's random_state, 1 or 2 false predictions made from both models
    
<br>

*Which one is better? Could you explain why?*

It depends on selected features. Decision tree model shows slightly better performance in accuracy level than random forest under certain conditions (well selected features set and certain random_state-under my base model setting result).  
    When I included forum activity(case 2), tree model performance decreased a lot, only 10 from 15 was correct. With same dataset random forest classifier shows same performance with 2 false predictions, forest is more stable to selected features(to false influence features), tree looks more sensitive.   
    Based on the feature importance data, random forest takes all features into consideration to predict, but tree model takes only few strongest ones. And three important features of forest model are actually correct considering how grades are given.   

In overall, I would claim that random forest is better.
    
<br>

1. Tools used:  
      - scikit-learn for solving classification problem and for visualizing confusion matrix and importance features  
       
      - classifier's setting is random_state=0(to get consistency from multiple runs) and all other parameters are default values
      
<br>
       
2. Confusion matrix:  

    *How are your model performing?*

    - forest classifier performance, the accuracy is about 0.867 (2 samples mismatching).  
    
    - current model could not predict label 2, our dataset has only 5 sample which has label 2, already 4 samples taken to train the model. Prediction probabilities of first sample data(false prediction) [0.02 0.19 0.5 0.25 0.04], it has low probability for label 3(false) of 0.50 for label 2(true) is 0.19. Another false prediction (10th sample) probatility distribution is [0. 0.01 0.46 0.45 0.08], proba of label 3(false) is 0.46 and the label 4(true) is 0.45, close each other. Both cases label 3 is predicted, precision score of grade 3 is very low. 
    
    - after injecting forum activity, the 10th data prediction probability drops for true label [0. 0. 0.51 0.42 0.07], proab difference between label 3 and 4 increased but accuracy level unchanged. 
    
    - tree classifier performed well with 1 false prediction(at 10th testing sample) but after injection of forum activity, performance in accuracy level is only 0.6667 (5 false prediction). With default parameter set(max_depth=None), tree does not predict probabilities for each label(1 or 0).  


    **Random forest model performance - classification report**  
    
   <img align="center" src="report.jpg" width="800" /> 
    
    
    **confusion matrix of two models**  
    
   <img src="case11.jpg" />



3. Important Features:  

    *What are the three most important features in predicting students’ final grade?*  
    
    - importance of each feature obtained using feature_importances_ method, sorting them in descending order and displaying in bar chart      
    - Random Forest model result: 3 mini-projects in order of Week7_MP3, Week5_MP2, Week3_MP1
    
    - which is correct based on the maximum points of each feature (MP3: 35, MP2: 20, MP1: 15) and all others are 5 points
    
    - On the other hand tree model puts feature of Week2_Quiz1 into a third position.  
    

    **Importance features of both models**  
   
   <img src="fi2.jpg"/>

<br>

*Do you need to change anything in your model?*
   
I have done some hyperparameter tuning for random forest classifier using Sklaern RandomSearch method, but I could not make the model better in accuracy point of view.  
I tried to split training and testing samples with different ratio and with differnt set(random state), with small number of samples hard to find optimum ratio.  
    

<br>


*What are the interesting observations you could report?*

The performance of model depending on classifier's random_state is slightly different was interesting. I would guess those two samples (first and 10th in testing samples), which both models could not predict well enough, might sit on grading boundary.    


##  Conclusion 
*what were the “scientific” bottlenecks? How did you overcome them?*

The one is feature selections, I see that importance of selected features from my short work and I know grading system (how to earn points and which column has bigger impact -we easily guess 3 important features based on course instruction without data analysis). I removed irrelevant features from my knowledge not using mathematical background between feature and label, so still there might be better selection of features for model to perform better which is left behind.   
I have tried including Week8_Total as one of feature, my decision tree model predicts perfectly(not always, some random state when data splitting), and tree used only one strongest feature of that column to make decision. On the other hand random forest could not make prediction perfectly but similar level of accuracy compared with using dataset without highly correlated column(Week8-total). I would say random forest classifier should be chosen. 

Another scientific bottlenect is small number of data and it is hard to overcom except collecting more data. To perform better for random forest classifier, I used cross-validation and parameter tuning technique (sklearn RandomizedSearchCV method), but I ended up same accuracy level with best estimator also (there might be some improvement point with random-state in model). With our limited number of dataset, I would say hard to achieve perfect prediction for unseen data. 





