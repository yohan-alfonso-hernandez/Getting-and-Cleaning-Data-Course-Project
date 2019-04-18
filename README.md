# Getting-and-Cleaning-Data-Course-Project
scripts for performing analysis of data

Getting and Cleaning Data Course Project by Yohan Alfonso Hernandez
This is a submission for Getting and Cleaning Data course project that contains the instruccions of transformation of Human Activity Recognition Using Smartphones Data Set 
oficial page= http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones#

Files:
Code book (CodeBook.md) that describes the data and transformations performed to clean up the data
run_analysis.R performs the data preparation and trnsformation  steps required as described in the course project’s definition:




## set working directory

setwd("C:\\Users\\Yohan\\Pictures\\Yohan\\Data sciece\\Getting data and cleanning data\\semana4\\Getting and Cleaning Data Course Project")


### Download files to local repository
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
file <- file.path(getwd(), "Dataset.zip")
download.file(url, file) 
unzip(file) 


What it's done after is prepare de data frames to work with

library(dplyr)


### Assigning dataframes
##first prepare the data downloaded
features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions"))
activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))
subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject")
subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject")


### Data sets  are:  (x_test and  y_test) , (x_train and y_train)

x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)
y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")

x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions)
y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")

##  A  glance to features and activities data frames

> head(features)
 # n         functions
 -1 tBodyAcc-mean()-X
 -2 tBodyAcc-mean()-Y
 -3 tBodyAcc-mean()-Z
 -4 tBodyAcc-std()-X
 -5 tBodyAcc-std()-Y
 -6 tBodyAcc-std()-Z


 >head(activities)
 # code   activity
 1      WALKING
 2      WALKING_UPSTAIRS
 3      WALKING_DOWNSTAIRS
 4      SITTING
 5      STANDING
 6      LAYING 


### 1 part Merges the training and the test sets to create one data set.

X <- rbind(x_train, x_test)                     # rbind makes one data.table from a list 
Y <- rbind(y_train, y_test)
Subject <- rbind(subject_train, subject_test)
MergedData <- cbind(Subject, Y, X)              # Merge data Combining Objects Y and X  by Rows or Columns with cbind

### 2 part Extracts only the measurements on the mean and standard deviation for each measurement.

TidyData <- MergedData %>% select(subject, code, contains("mean"), contains("std"))   # Select data that constains the word mean amd std


### 3 part Uses descriptive activity names to name the activities in the data set

TidyData$code <- activities[TidyData$code, 2]     # segregate  names of activities


### 4 part Appropriately labels the data set with descriptive variable names. 

names(TidyData)[2] = "activities"
names(TidyData)<-gsub("Acc", "Accelerometer", names(TidyData))          #  use gsub to change the names of the columns to a representative one
names(TidyData)<-gsub("Gyro", "Gyroscope", names(TidyData))
names(TidyData)<-gsub("BodyBody", "Body", names(TidyData))
names(TidyData)<-gsub("Mag", "Magnitude", names(TidyData))
names(TidyData)<-gsub("^t", "Time", names(TidyData))
names(TidyData)<-gsub("^f", "Frequency", names(TidyData))
names(TidyData)<-gsub("tBody", "TimeBody", names(TidyData))
names(TidyData)<-gsub("-mean()", "Mean", names(TidyData), ignore.case = TRUE)
names(TidyData)<-gsub("-std()", "STD", names(TidyData), ignore.case = TRUE)
names(TidyData)<-gsub("-freq()", "Frequency", names(TidyData), ignore.case = TRUE)
names(TidyData)<-gsub("angle", "Angle", names(TidyData))
names(TidyData)<-gsub("gravity", "Gravity", names(TidyData))

### 5 part  From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

AvgData <- TidyData %>%
        group_by(subject, activities) %>%               # group  subjectas and activities
        summarise_all(list(mean))                       # Use summarise_all to change  multiple columns

write.table(AvgData, "AvgData.txt", row.name=FALSE)   # writes data n a txt file


and final export the data to a file

write.table(AvgData, "AvgData.txt", row.name=FALSE)   # writes data n a txt file


