Getting and Cleaning Data Course Project CodeBook

This file describes the variables, the data and work that I have performed to clean up the data.




1. Read X_train.txt, y_train.txt and subject_train.txt from the "./data/train" folder and store them in x.train,y.train and subject.train.

2. Read X_test.txt, y_test.txt and subject_test.txt from the "./data/test" folder and store them in x.test,y.test,subject.test.

3. now in step-1 Merging the training and the test sets to create one data set (((using rbind because of no of coloums are same (checking using dim() function) ))) these are join_x,join_y and join_subject.

4.Read the features.txt file from the "/data" folder and store the data in a variable called features.

5. getting coloums which have mean() and std() at the end of features cloumn 2 using grep() and store them in mean_std. 

6.getting the coloumns of join_x corresponding to mean_std.
7.correcting names of join_x.

8.Read the activity_labels.txt file from the "./data"" folder and store the data in a variable called activities.

9.getting the coloumns of join_y corresponding to activities.
10.correcting name of join_y as activity.

11.correcting name of join_subject as subject.
12.binding all the data in single document called alldata.

13.Creating a second, independent tidy data set with the average of each column variable.67th and 68th coloumns are activity and subject respectively so we use onlt 1 to 66th column.


14.writing the tidy data set in file called tidydata_file.txt.


