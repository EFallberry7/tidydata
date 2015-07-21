#Getting and Cleaning Data
#Course Project Codebook

##The R script is found in the file ‘run_analysis.R’.

The data used for this project is the “Human Activity Recognition Using Smartphones Data Set”.  The data is described at
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
and can be obtained as
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

##Dataset description:
The data contains two sets, one called ‘Train’ and the other called ‘Test’, with these dimensions and file locations:
•	Train: 7,352 observations on 561 variables; stored in ‘x_train.txt’
•	Test:  2,947 observations on 561 variables:  stored in ‘x_test.txt’
The variable names are found in the file ‘features.txt’.  In addition to these variables, each observation corresponds to one of the 30 subjects in the study (numbered as integers 1-30), doing one of the 6 activities (numbered as integers 1-6).  For the Train data, the subjects are found in the file ‘subject_train.txt’ and activities are found in the file ‘y_train.txt’.  For the Test data, the subjects are found in the file ‘subject_test.txt’ and activities are found in the file ‘y_test.txt’.  

The steps below were completed to “tidy” the dataset and produce the smaller tidy dataset for the project:
1.	Use read.table to store the variable names in ‘features.txt’ in a dataframe called vnames.  This is a dataframe with 561 rows and 2 columns.  The first column is comprised of integers, with the second column comprised of the character names of the variables.

2.	Extract the second column of vnames and store the variable names in vnames2
3.	Create a 6 by 2 dataframe called act with the first column comprised of the integers 1 – 6, and the second column comprised of the items in the file ‘activity_labels.txt’.  For reference later, the second column is labeled atype.
4.	Use read.table to store the data from ‘x_train.txt’, in a dataframe called train.  In this read.table statement, use col.names = vnames2 to add the variable names to the dataframe columns. (Project item #4)
5.	Use read.table to store the activity numbers from ‘y_train.txt’ in trainL.
6.	Use read.table to store the subject numbers from ‘subject_train.txt’ in trainS.
7.	Use cbind to add the columns of activity and subject numbers to the data.  The result is a dataframe called testN.
8.	Merge testN and act by activity number and store in a dataframe called mact_train.  This step takes the activity numbers and assigns the corresponding character string activity for each observation.  (Project item #3)
9.	Repeat steps 4-8 for the test data.  In the code, the word ‘train’ is replaced with ‘test’ in both file references and new dataframe names.  This results in new dataframes test, testL, testS, testN, and mact_test.
10.	Use rbind to concatenate the mact_train and mact_test dataframes.  This merged dataframe is called mdata. (Project item #1)
11.	Use names to create a character vector of variable names from this merged dataset, called vnames3.
12.	Use a regular expression to extract columns for specific variables from mdata.  Need to extract the subject number (‘Subject’) and activity name (‘atype’).  Also need to extract all columns with ‘mean’ or ‘std’ in the name.  These columns are referenced using grep and the matching phrase “mean|std|atype|Subject” looked for in vnames3.  (i.e. mean or std or atype or Subject are found in a variable name in the vector vnames3).  The extracted columns are stored in a dataframe called mdata2. (Project item #2)
13.	Use aggregate to produce the smaller tidy dataset of means by subject and activity.  The variables are in columns 2-80 of mdata2.  The aggregate function calls for the calculation of the mean, by the two groups in columns Subject and atype.  The result is a dataframe called newdata. (Project item #5)
14.	Use write.table to store newdata in a text file ‘means.txt’.

