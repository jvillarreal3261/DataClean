# DataClean
Repository for week4 Assignment - Cleaning Data Course Project

This repository contains the script to tidy the data.


library(dplyr)
#step1 Read data
featuresname <- read.table("features.txt", stringsAsFactors = FALSE)
activitiesdescription <- read.table("activity_labels.txt", stringsAsFactors = FALSE)
trdata <- read.table("./train/X_train.txt", stringsAsFactors = FALSE)
tedata <- read.table("./test/X_test.txt", stringsAsFactors = FALSE)
tractivities <- read.table("./train/y_train.txt", stringsAsFactors = FALSE)
teactivities <- read.table("./test/y_test.txt", stringsAsFactors = FALSE)
tesubject <- read.table("./test/subject_test.txt", stringsAsFactors = FALSE)
trsubject <- read.table("./train/subject_train.txt", stringsAsFactors = FALSE)
#sept2 append both train,test, subjects and activites data in one
originDataTotal <- rbind(trdata, tedata)
activitiesTotal <- rbind(tractivities, teactivities)
subjectTotal <- rbind(trsubject, tesubject)
#step3 select only the mean and std colums from the features list
finalcolumnsId <- featuresname$V1[grepl('mean\\(\\)|std\\(\\)', featuresname$V2)]
datastep1 <- select(originDataTotal, finalcolumnsId)
selActivities <- activitiesdescription$V2[activitiesTotal$V1]
featurenames <- featuresname$V2[finalcolumnsId]
colnames(datastep1) <- featurenames
datastep1 <- cbind(datastep1, "subject" = subjectTotal$V1)
datastep2 <- cbind(datastep1, "activity" = selActivities)
#step4 generate the final dataset
finaldata <- datastep2 %>%
      group_by(activity, subject) %>%
      summarise_each(funs(mean))
#step5 generate the file
write.table(finaldata, "finaldata.txt", row.names = FALSE)

