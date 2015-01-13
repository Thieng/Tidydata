<<<<<<< HEAD
## file path of data file

path<-file.path(".","UCI HAR Dataset")

trainpath<-file.path(".","UCI HAR Dataset/train")
testpath<-file.path(".","UCI HAR Dataset/test")

##read data

X_train <- read.table(file.path(trainpath,"X_train.txt"), header=FALSE)
X_test <- read.table(file.path(testpath,"X_test.txt"), header=FALSE)

y_train <- read.table(file.path(trainpath,"y_train.txt"), header=FALSE)
y_test <- read.table(file.path(testpath,"y_test.txt"), header=FALSE)

subject_train <- read.table(file.path(trainpath,"subject_train.txt"), header=FALSE)
subject_test <- read.table(file.path(testpath,"subject_test.txt"), header=FALSE)


##combine data to one data set

train<-cbind(X_train,y_train,subject_train)
test<-cbind(X_test,y_test,subject_test)
data<-rbind(train,test)

##name data set

features <- read.table(file.path(path,"features.txt"), header=FALSE)
name<-c(as.character(features$V2),c("Activity","Subject"))
names(data)<-name

##Extracts only the measurements on the mean and standard deviation for each measurement

subdata<- subset(data,select=c(grep("mean\\(\\)|std\\(\\)",names(data),value=TRUE),"Activity","Subject"))

##Uses descriptive activity names to name the activities in the data set

activity <- read.table(file.path(path,"activity_labels.txt"), header=FALSE)
names(activity)<-c("id","activity")
data_activity<-merge(subdata,activity,by.x="Activity",by.y="id")
data_activity<-data_activity[,-1]

##Appropriately labels the data set with descriptive variable names

names(data_activity)<-gsub("^t", "time", names(data_activity))
names(data_activity)<-gsub("^f", "frequency", names(data_activity))
names(data_activity)<-gsub("Acc", "Acceleration", names(data_activity))
names(data_activity)<-gsub("Gyro", "Gyroscope", names(data_activity))
names(data_activity)<-gsub("Mag", "Magnitude", names(data_activity))
names(data_activity)<-gsub("BodyBody", "Body", names(data_activity))

##creates a second, independent tidy data set with the average of each variable for each activity and each subject.

tidy<- ddply(data_activity,.(activity,Subject),numcolwise(mean))
write.table(tidy,file="./tidy.txt")


=======
# Getting_cleaning_data
>>>>>>> dbb5e910007856076e2323e5744893614ffc7847
