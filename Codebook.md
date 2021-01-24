---
title: "Getting and Cleaning Data Assignment"
author: "aprandini"
date: "24/1/2021"
output: html_document
---

## 0 Reading training and test sets

```
subjectTrain <- read.table('./UCI HAR Dataset/train/subject_train.txt',header = F)
xTrain <- read.table('./UCI HAR Dataset/train/X_train.txt',header = F)
yTrain <- read.table('./UCI HAR Dataset/train/y_train.txt',header = F)

subjectTest <- read.table('./UCI HAR Dataset/test/subject_test.txt',header = F)
xTest <- read.table('./UCI HAR Dataset/test/X_test.txt',header = F)
yTest <- read.table('./UCI HAR Dataset/test/y_test.txt',header = F)

```

## 1 Merging training and test sets to create one data set

```
xData <- rbind(xTrain,xTest)
yData <- rbind(yTrain,yTest)
subjectData <- rbind(subjectTrain,subjectTest)

names(yData) <- c('activity')
names(subjectData) <- c('subject')
xDataNames <- read.table('./UCI HAR Dataset/features.txt',header = F)
names(xData) <- xDataNames$V2

allData <- cbind(xData,yData,subjectData)
```

## 2 Extracts only the measurements on the mean and standard deviation for each measurement

```
xData_mean_std <- xData[,grep('-(mean|std)\\(\\)',names(xData))]
allData_mean_std <- cbind(xData_mean_std,yData,subjectData)
```

## 3 Uses descriptive activity names to name the activities in the data set

```
activityLabels <- read.table('./UCI HAR Dataset/activity_labels.txt',header = F)
allData_mean_std[,ncol(allData_mean_std)-1] <- activityLabels[allData_mean_std[,ncol(allData_mean_std)-1],2]
```

## 4 Appropriately labels the data set with descriptive variable names

```
names(allData_mean_std) <- gsub('^t','time',names(allData_mean_std))
names(allData_mean_std) <- gsub('^f','frequency',names(allData_mean_std))
names(allData_mean_std) <- gsub('Acc','Accelerometer',names(allData_mean_std))
names(allData_mean_std) <- gsub('Gyro','Gyroscope',names(allData_mean_std))
names(allData_mean_std) <- gsub('Mag','Magnitude',names(allData_mean_std))
names(allData_mean_std) <- gsub('BodyBody','Body',names(allData_mean_std))
```

## 5 From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject

```
final <- aggregate(. ~subject + activity, allData_mean_std, mean)
final <- final[order(final$subject,final$activity),]
write.table(final, file = 'tidydata.txt', row.names = F)
```