# Practical Machine Learning Course Project

The purpose of this project was to build a prediction model to predict the "classe" variable.  Below is information regarding the source of the data used for this analysis.

Velloso, E.; Bulling, A.; Gellersen, H.; Ugulino, W.; Fuks, H. Qualitative Activity Recognition of Weight Lifting Exercises. Proceedings of 4th International Conference in Cooperation with SIGCHI (Augmented Human '13) . Stuttgart, Germany: ACM SIGCHI, 2013.

Read more: http://groupware.les.inf.puc-rio.br/har#wle_paper_section#ixzz3H7cGKa8a

First, I loaded the training and testing data files.  The testing file is actually for the final prediction and does not contain the "classe" variable.  So, in order to do cross validation, it was necessary to split the training data into training and test data sets.  I chose a 70/30 split using the "classe" variable.  70/30 is consistent with the splits used in some of the lectures.  
---

```
## Loading required package: lattice
## Loading required package: ggplot2
```
I then looked at a summary of the data.   The complete training set has 160 variables and 19622 observations.  I noticed that many variables had mostly NAs or blanks.  These variables would not be useful predictors.  I created new test and training data sets.  I looped through each of the variables and if the variable had a large number of NAs or blanks I did not add it to my new training and test data sets.  If the number of NAs or blanks was below a judgmentally determined threshold, then I added that variable to my new training and test sets.  This resulted in 53 variables remaining (including the outcome variable).  


I chose to use a random forest as my prediction model.  I used all the available predictor variables (52) in my new training data set. I trained the model using the new training data set and in order to perform cross-validation, I tested it on the new testing subset that I created.  Below is a table of actual vs. predicted classes.


```
## Loading required package: randomForest
## randomForest 4.6-10
## Type rfNews() to see new features/changes/bug fixes.
```

```
## rf variable importance
## 
##   only 20 most important variables shown (out of 52)
## 
##                      Overall
## roll_belt              100.0
## yaw_belt                79.5
## magnet_dumbbell_z       69.3
## magnet_dumbbell_y       64.2
## pitch_belt              63.1
## pitch_forearm           60.6
## magnet_dumbbell_x       51.7
## roll_forearm            50.2
## accel_dumbbell_y        45.5
## accel_belt_z            43.9
## magnet_belt_z           43.7
## roll_dumbbell           41.0
## magnet_belt_y           40.6
## accel_dumbbell_z        37.1
## roll_arm                33.4
## accel_forearm_x         32.7
## yaw_dumbbell            30.1
## total_accel_dumbbell    29.7
## accel_dumbbell_x        28.9
## accel_arm_x             28.1
```

```
## Random Forest 
## 
## 13737 samples
##    52 predictor
##     5 classes: 'A', 'B', 'C', 'D', 'E' 
## 
## No pre-processing
## Resampling: Bootstrapped (25 reps) 
## 
## Summary of sample sizes: 13737, 13737, 13737, 13737, 13737, 13737, ... 
## 
## Resampling results across tuning parameters:
## 
##   mtry  Accuracy  Kappa  Accuracy SD  Kappa SD
##    2    1         1      0.002        0.003   
##   27    1         1      0.002        0.003   
##   52    1         1      0.006        0.007   
## 
## Accuracy was used to select the optimal model using  the largest value.
## The final value used for the model was mtry = 2.
```

```
##     
## pred    A    B    C    D    E
##    A 1674    4    0    0    0
##    B    0 1134    0    0    0
##    C    0    1 1022   18    0
##    D    0    0    4  943    2
##    E    0    0    0    3 1080
```
One of the pro's of using random forests is the high accuracy.  So, I expected to the out-of-sample error rate to be low, but it was actually even lower than I was expecting.  The out-of-sample error rate is low (0.01). I was quite pleased with this result. Given time constraints and the good fit, I chose to stop with this rather than trying other methods and/or different combinations of variables.  


```
##  [1] B A B A A E D B A A B C B A E E A B B B
## Levels: A B C D E
```
Finally, I ran the official testing set through the model.  The predicted results are: B, A, B, A, A, E, D, B, A, A, B, C, B, A, E, E, A, B, B, B.  I expect that the out of sample error rate will be similar to what I find with the testing subset that I created and used previously (< 1%).  

