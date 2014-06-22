##This file describes the data in the data set, and describes the transformations necessary to get it

### So i have to say this is kind of broad. Im splitting this into 2 sections. the first section is a description of the dataset and the values in it. The second section is a description of the transformations necessary to get there

###section 1
The tidy data set has been output to tds.csv. There are 68 columns and 180 rows (not including the header row). The first column corresponds to a persons position while the measurement is being taken. The values corresponding to the "Testing Position column" are:
WALKING

WALKING_UPSTAIRS

WALKING_DOWNSTAIRS


SITTING


STANDING


LAYING



The second column corresponds to the actual person taking the test. Its a number from 1-30, an each person taking the tests has a unique number. There are 30 people, and there are 6 positions. So it makes sense that there are 30*6 rows.


The next 66 columns correspond to the actual measurement being taken. For examples, on column 9, the test is called tBodyAccJerk-mean()-X. For every person in all 6 positions, a measurement of this type was taken. Thats the same for all other 65 measurements. A list of all the features can be found in features.txt. A explanation of why the measurements are useful can be found in features_info.txt

###section 2

The tidy data set had to be crafted from the raw data. The raw data was the 3 files in /test, and the 3 files in /train. I joined each file in the test folder with the corresponding fil ein the train folder. I then joined those objects together into one big data.frame. I set the column names to the list of values in the features.txt file.
I then transformed the current object by subsetting based on if the measurements dealt with mean() or st(). I then aggregated by the person taking the test, and the position of the person taking the tets, and took the means on each measurement. Below is a description of each variable calcualted along the way:

subTrain--------Data.frame created by reading in the subject train file

subTest---------Data.frame created by reading in the subject test file

rawDataSubject--Data.frame created by joining the subTest and subTrain objects

YTrain----------Data.frame created by reading in the Y train file

YTest-----------Data.frame created by reading in the Y test file

rawDataY--------Data.frame created by joining the yTest and yTrain objects

XTest-----------Data.frame created by reading in the X test file

XTrain----------Data.frame created by reading in the X train file

rawDataX--------Data.frame created by joining the XTest and Xtrain objects

rawData---------Data.frame created by joining rawDataX rawDataY and rawDataSubject

v1--------------Vector created by using grep to get any column with mean() in the name

v2--------------Vector created by using grep to get any column with std() in the name

subsetRawData---Data.frame created by subseting rawData by v1 and v2 and the last 2 columns

newData---------Data.frame created by merging sybsetRawData with the output of features.txt

tds-------------Data.frame created by aggregating the newData by testPosition and testSubject, and retrieving means

tds.csv---------File created by writing tds to a file

