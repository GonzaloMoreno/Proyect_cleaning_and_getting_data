
#######################################################################################################################################################
#######################################################################################################################################################
######                                                  LIST OF INSTRUCTIONS                                                                       ####
######                                                                                                                                             ####
#######################################################################################################################################################
#######################################################################################################################################################
 





## declare library (plyr)

library(plyr)

## Set directory

specdata<-setwd ("C:/Users/GMORENO/Documents/R/cleaning data/")

## if it is needed, create a directory called "proyect"
if(!file.exists("proyecto")){
  
  dir.create("proyecto")  
  
}

## Copy link

arch_url<-"https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"

## Change directory

specdata<-setwd ("C:/Users/GMORENO/Documents/R/cleaning data/proyecto/")

## Download file

download.file(arch_url, destfile="./dataset.zip")

## read training and test files

training<- read.table(unz("dataset.zip", "UCI HAR Dataset/train/X_train.txt"))
test<- read.table(unz("dataset.zip", "UCI HAR Dataset/test/X_test.txt"))


## 1.Merges the training and the test sets to create one data set.

Datajoin<- join(x=test, y=training)

##2.Extracts only the measurements on the mean and standard deviation for each measurement.

total_data_mean<-subset(Datajoin, select=c(1:3, 41:43, 81:83, 121:123, 161:163, 201, 214,227, 240, 253, 266:268, 294:296,345:347,373:375,424:426,452:454,503,516,529, 542))
total_data_standard_desviation<-subset(Datajoin, select=c(4:6, 44:46, 84:86, 124:126, 164:166, 202,217,228, 241, 254, 269:271,348:350,427:429,504,517,530,543))

## 3.Uses descriptive activity names to name the activities in the data set

     ## Please to see codebook
x<-dim(Datajoin)
activity<- read.table(unz("dataset.zip", "UCI HAR Dataset/activity_labels.txt"))
activity2<-suppressWarnings(matrix(activity[,2], x[1], 1))
colnames(activity2)<-"Activity"
Datajoin<-cbind(Datajoin,activity2)

##4.Appropriately labels the data set with descriptive variable names.

nombre_columna<-read.table(unz("dataset.zip", "UCI HAR Dataset/features.txt"))
colnames(Datajoin)<-nombre_columna[,2]


## 5.Creates a second, independent tidy data set with the average of each variable for each activity and each subject.
## we are going to include de columns  with rhe 30 subjects

x<-dim(Datajoin)
subject<-rep(1:30, length.out=x[1])
subject<-matrix(subject, x[1],1)
colnames(subject)<-"Subject"
Datajoin2<-cbind(Datajoin,subject)

## Select de columns needed
mean_columns=c(1:3, 41:43, 81:83, 121:123, 161:163, 201, 214,227, 240, 253, 266:268, 294:296,345:347,373:375,424:426,452:454,503,516,529, 542)

## create new tody data

tidydata2 <-aggregate(Datajoin2[,mean_columns], by=list(Datajoin2[,562],Datajoin2[,563]), FUN=mean)


