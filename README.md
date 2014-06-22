## This is an overview of whats going on in the script
Note That step by step notes are in the actual run_analysis.R

###The first step is to import the raw data into R
As you can see in the run.analysis file, I went ahead and used read.table to read all the necessary
files into R. I then merged the traning files and the test files, and then ran cbind to make
one massive data.frame, which I called rawData. On line 34, i continue and I set the column names to 
what i extract from the features.txt file. I also add testPosition and testSubject to the end of it

###The fourth step was to appropriately labels the data set with descriptive variable names
Easy step. You just set the colnames of the current data set to the contents of the features.txt file. Since they
are already ordered in the file, you can just set colnames(newData) to the value as.vector(read.table"features.txt")
Make sure to add the testPosition and testSubject values, as they got lost on the merge last time

###The next step was to extract the mean and std measurements from the data set
To do that I used grep in order to creat vectors of the element locations where mean() and std() were being measured
I ended up with 66 measurements. I then subsetted the rawData and only kept the columns that corresponded to 
those vectors, and the testPosition and testSubject columns. 

###The next step was to use descriptive activity names to name the activities in the data set
That was an easy stp. All we do is merge on the subsetRawData we just created, and we merge it with the 
contents of the activity_labels.txt file. We make sure to merge using the correct variables, then save the output to 
newData.

##The final step was to create a second, independent tidy data set with the average of each variable for each activity and each subject. 
This was a touch one. The first part is to make testSubject a factor, since right now its a numeric. Then you want
to get the mean of every column, but have it sorted by testSubject and testPosition. tapply wont do it, you need to
use the aggregate function. Only aggregate the actual measurements, otherwise u get an error. Use testSubject and
V2(which corresponds to testPosition) and get the mean. Now set the column names for the first 2 columns, then
write the output to a csv file and dont forget to exclude row names

##Thats it. The run_analysis.R will give you the tidy data set in a csv file.


