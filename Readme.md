---
title: "Readme"
output: html_document
---

## Project work for the Course project - Getting and Cleaning the data

### Steps to create the tidy data set

1. load file 'run_analysis.R' into your R Studio. This file loads Data set directly from the [this URL](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip), so no need to load .zip file separately
2. give command 'source(run_analysis.R) in your R Studio to run the script
3. after run, a file called 'mydata2.txt' has been created into your working directory, which contains tidied data

### description of the script

First we load the data directly from the given URL and unzip the files under 'mydata' folder
```{r, eval=FALSE}

library(dplyr)
library(tidyr)
library(data.table)
library(reshape2)

myurl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(myurl,destfile="rawdata.zip")

if(!file.exists("./mydata")) {
  dir.create("./mydata")
}
unzip(zipfile="rawdata.zip", exdir = "./mydata")
```

Then we load the datafiles one by one and merge it to intermin tables called 'subject_data', 'activity_data' and 'our_data'

```{r, eval=FALSE}
subject_train <- tbl_df(read.table("mydata/UCI HAR Dataset/train/subject_train.txt"))
subject_test <- tbl_df(read.table("./mydata/UCI HAR Dataset/test/subject_test.txt"))

y_train <- tbl_df(read.table("./mydata/UCI HAR Dataset/train/Y_train.txt"))
y_test <- tbl_df(read.table("./mydata/UCI HAR Dataset/test/Y_test.txt"))

x_train <- tbl_df(read.table("./mydata/UCI HAR Dataset/train/X_train.txt"))
x_test <- tbl_df(read.table("./mydata/UCI HAR Dataset/test/X_test.txt"))

feature_data <- tbl_df(read.table("./mydata/UCI HAR Dataset/features.txt"))
activity_labels <- tbl_df(read.table("./mydata/UCI HAR Dataset/activity_labels.txt"))

##now after we have loaded the data files to R, we start first by combining all the files to one data table
subject_data <- rbind(subject_train, subject_test)
activity_data <- rbind(y_train,y_test)
our_data <- rbind(x_train,x_test)

```

Now we rename some column names to be compliant with tidy data requirements


```{r, eval=FALSE}

##  now we rename some of the column names based on specification to comply more on tidy data principles
setnames(subject_data, "V1", "subjectID")
setnames(activity_data,"V1","activityNum")
setnames(feature_data,names(feature_data),c("featureNum","featureName"))
colnames(our_data) <- feature_data$featureName
setnames(activity_labels,names(activity_labels),c("activityNum","activityName"))

```

now we can combine these all data files to one data table (tbl_df) named 'mydata',which is our final combined data table

```{r,eval=FALSE}
mydata <- tbl_df(cbind(cbind(subject_data,activity_data),our_data))
```

Now when we have all data in mydata table, we extract only mean and std columns to be used

```{r, eval=FALSE}
meanstdcols <- grepl("mean\\(\\)", names(mydata)) | grepl("std\\(\\)", names(mydata))
meanstdcols[1:2] <- TRUE
mydata <- mydata[,meanstdcols]
```

Then we rename columns to be more meaningful

```{r, eval=FALSE}
names(mydata)<-gsub("std()", "std", names(mydata))
names(mydata)<-gsub("mean()", "mean", names(mydata))
names(mydata)<-gsub("^t", "time", names(mydata))
names(mydata)<-gsub("^f", "frequency", names(mydata))
names(mydata)<-gsub("Acc", "Accelerometer", names(mydata))
names(mydata)<-gsub("Gyro", "Gyroscope", names(mydata))
names(mydata)<-gsub("Mag", "Magnitude", names(mydata))
names(mydata)<-gsub("BodyBody", "Body", names(mydata))

```

finally we create a new txt.file called 'mydata2.txt', which is tidied version of the data.zip originally loaded.
We use melt and dcast functions to calculate average of each variable for each activity and each subject

```{r, eval=FALSE}

 melted <- melt(mydata,id=c("subjectID","activityNum"))
 mydata2 <- dcast(melted,subjectID+activityNum ~ variable,mean)
 write.table(mydata2,"mydata2.txt", row.name=FALSE, sep=";")


```
 

