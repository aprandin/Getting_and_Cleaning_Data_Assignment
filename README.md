## Getting_and_Cleaning_Data_Assignment

##### _Reading training and test sets_

To run the code, be sure that your working directory contains the 'UCI HAR Dataset' folder.

##### _Merging training and test sets to create one data set_

Datasets for, respectively, the x, y and subject data are created. These datasets are merged in a dataframe called 'allData'.

##### _Extracts only the measurements on the mean and standard deviation for each measurement_

Here, we consider only the measurements for the mean and standard deviation columns.

##### _Uses descriptive activity names to name the activities in the data set_

Here, we assign to the activity variables the respective names from the 'activity_labels.txt' file.

##### _Appropriately labels the data set with descriptive variable names_

Here, we rename columns' names according to the 'features_info.txt' file (eg. prefix 't' denotes time, prefix 'f' denotes frequency,...)

##### _From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject_

Finally, we create an indipendent tidy data set file called 'tidydata.txt'.