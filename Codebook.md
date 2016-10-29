---
title: "Codebook.md"
author: "John Palmer"
date: "October 29, 2016"
output: html_document
---

# Getting and Cleaning Data Course Project
Author: John Palmer</p>
Date: October 29, 2016</p>
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. 
The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on 
a series of yes/no questions related to the project. 


You will be required to submit: 
  1) a tidy data set as described below, 
  2) a link to a Github repository with your script for performing the analysis, and 
  3) a code book that describes the variables, the data, and any transformations or work 
    that you performed to clean up the data called CodeBook.md. You should also include a README.md in the 
    repo with your scripts. This repo explains how all of the scripts work and how they are connected.

Here is the data for the project:
  https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

Create one R script called run_analysis.R that does the following.

0) Download and prepare data.
1) Merges the training and the test sets to create one data set.
2) Extracts only the measurements on the mean and standard deviation for each measurement.
3) Uses descriptive activity names to name the activities in the data set
4) Appropriately labels the data set with descriptive variable names.
5) From the data set in step 4, creates a second, independent tidy data set with the average of each variable 
   for each activity and each subject.


Step 0. Initial Preparation - 
Ensure data.table library is loaded
Set the file URL to the dataset
Download the zipped data set if it does not exist
Unzip the data set into the UCI HAR Dataset directory

<code> library(data.table)
fileurl = 'https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip'
setwd('D:/My Documents/Education/Coursera/Data-Cleaning/Week-4/')
if (!file.exists('./UCI HAR Dataset.zip')){
  download.file(fileurl,'./UCI HAR Dataset.zip', mode = 'wb')
  unzip("UCI HAR Dataset.zip", exdir = '.')
}</code>

Prepare the data
Read in the 561 feature labels 
The complete list of variables of each feature vector is available in 'features.txt'
```features <- read.csv('./UCI HAR Dataset/features.txt', header = FALSE, sep = ' ')
features <- as.character(features[,2])```

Prepare data tables from the subject_train, X_train, and y_train files   
```data.train.x <- read.table('./UCI HAR Dataset/train/X_train.txt')
data.train.activity <- read.csv('./UCI HAR Dataset/train/y_train.txt', header = FALSE, sep = ' ')
data.train.subject <- read.csv('./UCI HAR Dataset/train/subject_train.txt',header = FALSE, sep = ' ')```

Create a new data.frame named data.train from data.train.subject, data.train.activity, data.train.x
```data.train <-  data.frame(data.train.subject, data.train.activity, data.train.x)```

Store a list of names in data.train which refer to each column of the data.train.x, data.train.activity, data.train.subject from the features vector
```names(data.train) <- c(c('subject', 'activity'), features)```

Prepare test tables from the subject_train, X_train, and y_train files  
```data.test.x <- read.table('./UCI HAR Dataset/test/X_test.txt')
data.test.activity <- read.csv('./UCI HAR Dataset/test/y_test.txt', header = FALSE, sep = ' ')
data.test.subject <- read.csv('./UCI HAR Dataset/test/subject_test.txt', header = FALSE, sep = ' ')```

Create a new data.frame named data.test from data.test.subject, data.test.activity, data.test.x
```data.test <-  data.frame(data.test.subject, data.test.activity, data.test.x)```

Store a list of names in data.test which refer to each column of the data.test.x, data.test.activity, data.train.test from the features vector
```names(data.test) <- c(c('subject', 'activity'), features)```


Step 1. Merges the training and the test sets to create one data set named data.all
Merges the training and the test sets prepared in Step 0 to create a single data set called data.all
```data.all <- rbind(data.train, data.test)```

Step 2. Extracts only the measurements on the mean and standard deviation for each measurement.
```col.select <- grep('mean|std', features)
data.sub <- data.all[,c(1,2,col.select + 2)]```

Step 3. Uses descriptive activity names to name the activities in the data set
```activity.labels <- read.table('./UCI HAR Dataset/activity_labels.txt', header = FALSE)
activity.labels <- as.character(activity.labels[,2])
data.sub$activity <- activity.labels[data.sub$activity]```

Step 4. Appropriately label, through substitution, the data set with descriptive variable names.
```name.new <- names(data.sub)
name.new <- gsub("[(][)]", "", name.new)
name.new <- gsub("^t", "TimeDomain_", name.new)
name.new <- gsub("^f", "FrequencyDomain_", name.new)
name.new <- gsub("Acc", "Accelerometer", name.new)
name.new <- gsub("Gyro", "Gyroscope", name.new)
name.new <- gsub("Mag", "Magnitude", name.new)
name.new <- gsub("-mean-", "_Mean_", name.new)
name.new <- gsub("-std-", "_StandardDeviation_", name.new)
name.new <- gsub("-", "_", name.new)
names(data.sub) <- name.new```

Step 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable 
   for each activity and each subject.
```data.tidy <- aggregate(data.sub[,3:81], by = list(activity = data.sub$activity, subject = data.sub$subject),FUN = mean)
write.table(x = data.tidy, file = "data_tidy.txt", row.names = FALSE)```

