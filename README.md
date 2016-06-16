##import some requested module
library(dplyr)
library(reshape2)

##The X  Y subject data from file
##I have save the file to ./data/Dataset
##Because the file's name is too long >_<
x_test <- read.table("./data/Dataset/test/X_test.txt")
y_test <- read.table("./data/Dataset/test/y_test.txt")
x_train <- read.table("./data/Dataset/train/X_train.txt")
y_train <- read.table("./data/Dataset/train/y_train.txt")
subject_test <- read.table("./data/Dataset/test/subject_test.txt")
subject_train <- read.table("./data/Dataset/train/subject_train.txt")
activity_label <- read.table("./data/Dataset/activity_labels.txt")

##Get the Name and Combin the test and train
features <- read.table("./data/Dataset/features.txt")
allx <- rbind(x_test,x_train)
names(allx) <- features[,2]


##Merges the label and 
##Uses descriptive activity names to name the activities in the data set
alllabel <- rbind(y_test,y_train)
alllabel$V2 <- activity_label[alllabel$V1,"V2"]

##Extracts only the measurements on the mean and standard deviation for each measurement.
all <- allx[,grep("mean\\(\\)|std\\(\\)",names(allx))]

##Appropriately labels the data set with descriptive variable names.
names(all)<-gsub("std\\(\\)", "Std", names(all))
names(all)<-gsub("mean\\(\\)", "Mean", names(all))
names(all)<-gsub("^t", "Time", names(all))
names(all)<-gsub("^f", "Frequency", names(all))
names(all)<-gsub("Acc", "Accelerometer", names(all))
names(all)<-gsub("Gyro", "Gyroscope", names(all))
names(all)<-gsub("Mag", "Magnitude", names(all))
names(all)<-gsub("BodyBody", "Body", names(all))
names(all)<-gsub("-","",names(all))

##Merge all the data
all$activity <- alllabel$V2
all$subject <- rbind(subject_test,subject_train)$V1

##creates a second, independent tidy data set with the average of 
#each variable for each activity and each subject.
melted <- melt(all,id=c("activity","subject"))
ans <- dcast(melted,subject+activity ~ variable,mean)

##Save to the file
write.table(ans, file = "tidydata.txt", row.name=FALSE)