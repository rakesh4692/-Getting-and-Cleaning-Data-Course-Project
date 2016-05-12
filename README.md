# -Getting-and-Cleaning-Data-Course-Project
###################################################################################
# SETTING WORKING DIRECTORY TO FOLDER *********WHICH CONTAINS THIS***** UCI HAR Dataset****** folder 
# i.e working directory is """NOT UCI HAR Dataset"""


x.train<-read.table("./UCI HAR Dataset/train/X_train.txt")
head(x.train)
dim(x.train)
## [1] 7352  561

y.train<-read.table("./UCI HAR Dataset/train/y_train.txt")
head(y.train)
dim(y.train)
## [1] 7352    1

subject.train<-read.table("./UCI HAR Dataset/train/subject_train.txt")
head(subject.train)
dim(subject.train)
## [1] 7352    1

x.test<-read.table("./UCI HAR Dataset/test/X_test.txt")
head(x.test)
dim(x.test)
## [1] 2947  561

y.test<-read.table("./UCI HAR Dataset/test/y_test.txt")
head(y.test)
dim(y.test)
## [1] 2947    1

subject.test<-read.table("./UCI HAR Dataset/test/subject_test.txt")
head(subject.test)
dim(subject.test)
## [1] 2947    1




#############################################################################################
## step-1 
## Merges the training and the test sets to create one data set
join_x<-rbind(x.train,x.test)
dim(join_x)
## [1] 10299   561
join_y<-rbind(y.train,y.test)
dim(join_y)
## [1] 10299     1
join_subject<-rbind(subject.train,subject.test)
dim(join_subject)
## [1] 10299     1





############################################################################################
## Step 2
## Extract only the measurements on the mean and standard deviation for each measurement

features<-read.table("./UCI HAR Dataset/features.txt")
## dim(features)
## [1] 561   2

## head(features)
## 1  1 tBodyAcc-mean()-X
## 2  2 tBodyAcc-mean()-Y
## 3  3 tBodyAcc-mean()-Z
## 4  4  tBodyAcc-std()-X
## 5  5  tBodyAcc-std()-Y
## 6  6  tBodyAcc-std()-Z
## get only which have mean() and std() at the end of features cloumn 2


mean_std<-grep("-(mean|std)\\(\\)",features[,2])
## class(mean_std)
## [1] "integer"
## length(mean_std)
## [1] 66

## getting the coloumns of join_x which have mean() and std() at the end 
join_x<-join_x[,mean_std]

## dim join_x
## [1] 10299    66
## head(names(join_x))
## [1] "V1" "V2" "V3" "V4" "V5" "V6"

## correcting names of join_x
names(join_x)<-features[mean_std,2]

## head(names(join_x))
## [1] "tBodyAcc-mean()-X" "tBodyAcc-mean()-Y" "tBodyAcc-mean()-Z" "tBodyAcc-std()-X" 
## [5] "tBodyAcc-std()-Y"  "tBodyAcc-std()-Z"




#############################################################################################
## Step 3
## Use descriptive activity names to name the activities in the data set

activities<-read.table("./UCI HAR Dataset/activity_labels.txt") 

## str(activities)
## 'data.frame':	6 obs. of  2 variables:
## $ V1: int  1 2 3 4 5 6
## $ V2: Factor w/ 6 levels "LAYING","SITTING",..: 4 6 5 2 3 1

## update values with correct name 
join_y[,1]<-activities[join_y[,1],2]

## head(activities[join_y[,1],2])
## [1] STANDING STANDING STANDING STANDING STANDING STANDING
## Levels: LAYING SITTING STANDING WALKING WALKING_DOWNSTAIRS WALKING_UPSTAIRS







############################################################################################
## Step 4
## Appropriately label the data set with descriptive variable names

# correct column name
names(join_y) <- "activity"


## correct column name
names(join_subject)<-"subject"

## binding all the data in single document
alldata<-cbind(join_x,join_y,join_subject)

## dim(alldata)
## [1] 10299    68    i.e 66+2=68


############################################################################################
## Step 5
## Create a second, independent tidy data set with the average of each variable


## loading required library
library(plyr)

## 67th and 68th coloumns are activity and subject respectively
tidydata<-ddply(alldata,.(activity,subject),function(x) colMeans(x[,1:66]))

## writing the tidy data set
write.table(tidydata,"tidydata_file.txt",col.names = TRUE,row.names = FALSE)


