# tidydata

## Getting and Cleaning Data
## Course project

##### read in variable names into a k x 2 dataframe
##### then extract column 2 for use in read.table statements
vnames <- read.table("features.txt")  
vnames2 <- vnames[,2]  

####create 6 x 2 dataframe with activity numbers and activity names
#####for use below when assigning activity names to the dataset
anum <- 1:6
atype <- c("WALKING","WALKING_UPSTAIRS","WALKING_DOWNSTAIRS","SITTING"
           ,"STANDING","LAYING")

act <- data.frame(anum, atype)

##### read train dataset into a dataframe using variable names 
##### then read activity numbers in y_train.txt
##### then read subject numbers in subject_train.txt
##### then cbind activity and subject numbers with dataset
##### then merge activity names with dataset
train <- read.table("train/x_train.txt", col.names = vnames2)

trainL <- read.table("train/y_train.txt", col.names = "Activity")

trainS <- read.table("train/subject_train.txt", col.names = "Subject")

trainN <- cbind(trainL,trainS,train)

mact_train = merge(trainN,act,by.x="Activity",by.y="anum")

##### repeat steps for dataset test
test <- read.table("test/x_test.txt", col.names = vnames2)

testL <- read.table("test/y_test.txt", col.names = "Activity")

testS <- read.table("test/subject_test.txt", col.names = "Subject")

testN <- cbind(testL,testS,test)

mact_test = merge(testN,act,by.x="Activity",by.y="anum")

##### stack (concatentate) the two datasets using rbind
mdata <- rbind(mact_test,mact_train) 

##### create vector of varible names from mdata
##### use regular expression to find column names with "mean" or "std"
##### select all rows and only those columns
vnames3 <- names(mdata)

mdata2 <- mdata[,grep("mean|std|atype|Subject",vnames3)]

##### compute means of the variables by subject and activity
##### then write the reulting dataframe into a text file
newdata <- aggregate(mdata2[,2:80],list(Subject = mdata2$Subject, 
                                        Activity = mdata2$atype), mean)

write.table(newdata, file = "means.txt", row.name = FALSE)

