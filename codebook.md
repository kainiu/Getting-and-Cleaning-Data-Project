##CODEBOOK

###describe the variables, data and transformation

###First the data source is 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
A full description is available at the site where the data was obtained:
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

###In original data

it copy from the description of the dataset


The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

The dataset includes the following files:
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


###What I change it

As requested, I do some work to the dataset.

1.Merges the training and the test sets to create one data set.
	I merge the training and the test sets. And the subject is from merged "test/subject_test.txt" and "train/subject_train.txt"
	activity from merged "test/Y_test.txt" and "train/Y_train.txt"
	The sets is saved to variable all

2.Extracts only the measurements on the mean and standard deviation for each measurement.
	Use grep to extract the names of sets which have mean() or std()

3.Uses descriptive activity names to name the activities in the data set
	The variables activity_label is save the activity names.
	And then it is merged to the variables alllabel by V1

4.Appropriately labels the data set with descriptive variable names.
	I do it like this:

	names(all)<-gsub("std\\(\\)", "Std", names(all))
	names(all)<-gsub("mean\\(\\)", "Mean", names(all))
	names(all)<-gsub("^t", "Time", names(all))
	names(all)<-gsub("^f", "Frequency", names(all))
	names(all)<-gsub("Acc", "Accelerometer", names(all))
	names(all)<-gsub("Gyro", "Gyroscope", names(all))
	names(all)<-gsub("Mag", "Magnitude", names(all))
	names(all)<-gsub("BodyBody", "Body", names(all))
	names(all)<-gsub("-","",names(all))

5.From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
	melted <- melt(all,id=c("activity","subject"))
	ans <- dcast(melted,subject+activity ~ variable,mean)

###Save to file

write.table(ans, file = "tidydata.txt", row.name=FALSE)
