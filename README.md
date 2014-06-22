# I have line by line notes in the actual run_analysis
# this is more of an overview of whats going on in the script


#read the subject_train file and the subject test file
subTrain<-read.table("train/subject_train.txt")
subTest<-read.table("test/subject_test.txt")

#merge them into one variable. This will contain all the subject ids.
#So basically, each person has a number from 1 to 30
# This file has that so we can map who got what scores
rawDataSubject<-rbind(subTrain,subTest)

#read the y train and test files
YTrain<-read.table("train/y_train.txt")
YTest<-read.table("test/y_test.txt")

#merge them into one variable. This will contains the poistions in which they took the tests 
# i.e. if they get a 1, it means they were walking 
# notice its exactly the same size as the size of the subjects.
rawDataY<-rbind(YTrain,YTest)

# read the x train and test files
XTest<-read.table("test//X_test.txt",fill=TRUE)
XTrain<-read.table("train//X_train.txt",fill=TRUE)

# merge them. This contains the results of the particular test. So on column 454, they will have a numeric
# such as .014556 That means that for test 454 (fBodyGyro-meanFreq()-Z), they got a .014556
rawDataX<-rbind(XTrain,XTest)

#bind them together in one data set
rawData<-cbind(rawDataX,rawDataY,rawDataSubject)

names1<-read.table("features.txt")[,2]
#set the column names. this will give us the descriptive variable names
colnames(rawData)<-c(as.vector(read.table("features.txt")[,2]),"testPosition","testSubject")

#remove the temp variables from memory
rm(list=c("subTest","subTrain","XTest","XTrain","YTest","YTrain","rawDataSubject","rawDataX","rawDataY"))

#So there are 561 "features" in the rawData set now. We also have the "test position" and
#the "testSubject". The test subject is the person (there are 30 people and each as a number). The testPostion
# is 1:6, and corresponds to standing, sitting walking, etc

#We only care about the features (the directoins refer to them as measurements) that compute the mean
# and the standard deviation. To do that, we will use grep.
#We will grep the patterns "mean()" and "std()" from the colnames of rawData and save to v1 and v2
# fixed=TRUE so it handles the ()

v1<-grep("mean()",colnames(rawData),fixed=TRUE)
v2<-grep("std()",colnames(rawData),fixed=TRUE)

# this will give us a subset of the data with just the measurements we care about, and the testPosition and
# testSubject columns
subsetRawData<-rawData[,c(v1,v2,562,563)]

#clean up to save memory
rm(rawData)

#now we need to name the activities in the data set. we can get those names from the activity_labels.txt
# we will then merge with the subsetRawData
newData<-merge(subsetRawData,read.table("activity_labels.txt"),by.x="testPosition",by.y="V1")

#clean up the subsetrawData
rm(subsetRawData)

# now we want the average of each variable for each activity and each subject

#will set the testSubject variable to a factor.same for 
newData$testSubject<-as.factor(newData$testSubject)

# the following does it for one variable. gives me 180 rows. will need to do this for all variables
#tapply(newData[,4],list(newData$V2,newData$testSubject),mean)

# we just modify the tapply and use aggregate. we only aggregate on the measurements we care about
# so we use newData[,2:67] not newData
tds<-aggregate(newData[,2:67],list(newData$V2,newData$testSubject),mean)
# ill rename the first 2 columns really quick
colnames(tds)[1:2]<-c("Testing Position","Test Subject")
#now ill save the tidy data set to a csv file. wont use row names
write.csv(tds,file="tds.csv",row.names=FALSE)