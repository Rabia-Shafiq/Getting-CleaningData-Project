# Project
==================================================================
Human Activity Recognition Using Smartphones Dataset
# Data explanation
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

For each record it is provided:
======================================

- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.
- The dataset includes the following files:
=========================================

- 'README.txt'

- 'features_info.txt': Shows information about the variables used on the feature vector.

- 'features.txt': List of all features.

- 'activity_labels.txt': Links the class labels with their activity name.

- 'train/X_train.txt': Training set.

- 'train/y_train.txt': Training labels.

- 'test/X_test.txt': Test set.

- 'test/y_test.txt': Test labels.

The following files are available for the train and test data. Their descriptions are equivalent. 

- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 

- 'train/Inertial Signals/total_acc_x_train.txt': The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows a 128 element vector. The same description applies for the 'total_acc_x_train.txt' and 'total_acc_z_train.txt' files for the Y and Z axis. 

- 'train/Inertial Signals/body_acc_x_train.txt': The body acceleration signal obtained by subtracting the gravity from the total acceleration. 

- 'train/Inertial Signals/body_gyro_x_train.txt': The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second. 
# VARIABLES:
After loading the data set Column name(variables) was given to train and test set. And train and test set was merged on basis of same column names. Variables of merged data set are as:

* Activity_Label
* Subject
* timeBodyAccEMean-X
* timeBodyAccMean-Y
* timeBodyAccMean-Z
* timeBodyAcc-std-X
* timeBodyAcc-std-Y
* timeBodyAcc-std-Z
* timeBodyAcc-mad-Y
* .................
* ...............
* Last variables...
* angle(Y,GravityMean)
* angle(Z,GravityMean)

# Transformations for cleaning up data

## 1. Merges the training and the test sets to create one data set.
###setting working directory to project folder where UCI HAR Dataset was unzipped.
setwd("C://MyStuf/Rabiz-Data/JohnHopkins/Getting&CleaningData/getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset")

###Reading data from files
features     = read.table('./features.txt',header=FALSE);

###Reading train data

*subjectTrain = read.table('./train/subject_train.txt',header=FALSE);
*xTrain       = read.table('./train/x_train.txt',header=FALSE);
* yTrain       = read.table('./train/y_train.txt',header=FALSE); 

### Assigning column names to imported training data

colnames(subjectTrain)  = "Subject";

### checking featuers columns

* colnames(features);
* features[,2];
 
### checking xTrain columns

colnames(xTrain);

### Assigning xTrain column names as its equavalent to features[,2]number of names

* colnames(xTrain) = features[,2]  ;
* colnames(yTrain)        = "Activity_Label"

###Create the final training set by merging xTrain,yTrain,subjectTrain
trainingData=cbind(subjectTrain,yTrain,xTrain)

###Reading test data

* subjectTest = read.table('./test/subject_test.txt',header=FALSE); 
* xTest       = read.table('./test/x_test.txt',header=FALSE); 
* yTest       = read.table('./test/y_test.txt',header=FALSE); 

### Assigning column names to imported test data

* colnames(subjectTest)= "Subject"
* colnames(xTest) = features[,2]  ; 
* colnames(yTest)="Activity_Label"

### Create the final Test set by merging xTest,yTest,subjectTest

testData = cbind(subjectTest,yTest,xTest)
## Merging Training and test set to create final data set
finalDataSet= rbind(trainingData,testData)

## 2. Uses descriptive activity names to name the activities in the data set

activityType = read.table('./activity_labels.txt',header=FALSE); #imports activity_labels.txt
### As activity having total 6 variables and yTrain,yTest named to 'Label' also having values 1-6 so following command assigning names of 1-6 activities e.g walking, sitting, laying in final data set.
*finalDataSet$Activity_Label<-factor(finalDataSet$Activity_Label,levels=activityType$V1,labels=activityType$V2)

## 3. Appropriately labels the data set with descriptive variable names. 
colNames=colnames(finalDataSet)
for (i in 1:length(colNames)) 
{
 * colNames[i] = gsub("\\()","",colNames[i])
*  colNames[i] = gsub("-std$","StdDev",colNames[i])
 * colNames[i] = gsub("-mean","Mean",colNames[i])
  * colNames[i] = gsub("^(t)","time",colNames[i])
  * colNames[i] = gsub("^(f)","freq",colNames[i])
  * colNames[i] = gsub("([Gg]ravity)","Gravity",colNames[i])
  * colNames[i] = gsub("([Bb]ody[Bb]ody|[Bb]ody)","Body",colNames[i])
 ..........................................................
* colNames[i] = gsub("GyroMag","GyroMagnitude",colNames[i])
};
Here change the -std to StdDev, mean to Mean,  't' to time, 'f' to freq in data variables.






