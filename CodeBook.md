	


The script  run_analysis.R script performs the data tidy in 5 steps required in the coursera course Getting and Cleaning Data Course Project

Download files to local repository
The dataset is  downloaded compresses and extracted under the folder called UCI HAR Dataset
contains a data of a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities.

Assigning dataframes
features : 561 rows, 2 columns 
The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ.
a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag.

Activities: 6 rows, 2 columns 
List of activities performed when the corresponding measurements were taken and its codes 
1 WALKING
2 WALKING_UPSTAIRS
3 WALKING_DOWNSTAIRS
4 SITTING
5 STANDING
6 LAYING

subject_test: 2947 rows, 1 variable contains data of  the 30% of voluntaries. 
subject_train : 7352 rows, 1 variable contains  data of  70% of the volunteers.

Data sets  are:  (x_test and  y_test) , (x_train and y_train)

x_test : 2947 rows, 561 columns  contains recorded features test data label = functions
y_test : 2947 rows, 1 variable  contains test data of activities label =code
x_train <- test/X_train.txt : 7352 rows, 561 variables contains  train data of activities label = functions
y_train <- test/y_train.txt : 7352 rows, 1 variables contains train data of activities label =code

1 Merges the training and the test sets to create one data set
Data frame MergeData: 10299 obs  of 563 variables is created Merging data Combining Objects Y and X  from test and train by Rows or Columns with cbind 

2 Extracts only the measurements on the mean and standard deviation for each measurement
Data frame TidyData: 10299 obs  of 88 variables  is created by subsetting Merged_Data, selecting only columns: subject, code that contain mean and standard deviation (std) for each measurement

3 Uses descriptive activity names to name the activities in the data set
subset TidyData$Code Segregate  numbers in code column of the TidyData replaced with corresponding activity taken from second column of the activities variable

4 Appropriately labels the data set with descriptive variable names
 gsub is used to change the names of the columns to a representative one
the code column in TidyData renamed into activities
All Acc in column’s name replaced by Accelerometer
All Gyro in column’s name replaced by Gyroscope
All BodyBody in column’s name replaced by Body
All Mag in column’s name replaced by Magnitude
All mean in columns name replace by Mean
All std in columns name replace by STD
All freq in columns name replace by Frecuency
All start with character f in column’s name replaced by Frequency
All start with character t in column’s name replaced by Time
All angle in columns name replace by Angle
All gravity in columns name replace by Gravity


5 From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject
Data frae: AvgData  180 rows, 88 variables  is created by sumarizing TidyData taking the means of each variable for each activity and each subject, after groupped by subject and activity.

Export FinalData into AvgData.txt file.


