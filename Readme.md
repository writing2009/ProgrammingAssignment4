Getting and Cleaning Data Course Project
========================================

John Palmer

October 29, 2016

## Project Description
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set.
The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers
on a series of yes/no questions related to the project.

Here are the data for the project: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

You should create one R script called run_analysis.R that does the following.

* Merges the training and the test sets to create one data set.
* Extracts only the measurements on the mean and standard deviation for each measurement. 
* Uses descriptive activity names to name the activities in the data set
* Appropriately labels the data set with descriptive activity names. 
* Creates a second, independent tidy data set with the average of each variable for each activity and each subject. 

## What you find in this repository

* __Codebook.md__: information about raw and tidy data set and elaboration made to transform them
* __Readme.md__: this file
* __run_analysis.R__: R script to transform raw data set in a tidy one
* __tidy_data.txt__: The tidy_data.txt file

## How to create the tidy data set

1. open a R console 
2. source run_analysis.R script: `source('run_analysis.R')`

In the repository root directory you find the file `tidy_data.txt` with the tidy data set.