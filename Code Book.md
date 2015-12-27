---
title: "code book"
output: html_document
---

## Code book for tidy data of the Project work

This is the code book describing the data output, and how to interpret the results.

## Overview of the data

30 volunteers performed 6 different activities while wearing a Samsung smartphone. Various data about their movements were recorded and stored to data capture files. The content of these
data files are described in this code book.

## Details of files

* 'features.txt' 		- this file contains the names for the 561 features in the smartphone
* 'activity_labels.txt' 	- this file contains the Names and IDs for each of the 6 activities
* 'X_train.txt'			- this file contains 7352 observations of the 561 features, for 21 of the 30 volunteers
* 'subject_train.txt' 		- this file contains a vector of 7352 integers, denoting the ID of the volunteer related to each of the observations in 'X_train.txt'
* 'y_train.txt'			- this file contains a vector of 7352 integers, denoting the ID of the volunteer related to each of the observations in 'X_train.txt'
* 'X_test.txt'			- this file contains 2947 observations of the 561 features, for 9 of the 30 volunteers
* `subject_test.txt`: 		- this file contains a vector of 2947 integers, denoting the ID of the volunteer related to each of the observations in `X_test.txt`.
* `y_test.txt`			- this file contains a vector of 2947 integers, denoting the ID of the activity related to each of the observations in `X_test.txt`.

you can get more details on the 'feature_info.txt' file which is located in the data package.

## Processing steps

*    Merge the training and the test sets to create one data set.
*    Extract only the measurements on the mean and standard deviation for each measurement. 
*    Use descriptive activity names to name the activities in the data set
*    Appropriately label the data set with descriptive variable names. 

