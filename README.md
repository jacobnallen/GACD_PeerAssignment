# GACD_PeerAssignment
Getting and Cleaning Data Peer Assignment - Coursera Data Science

## 1: Download the dataset
## 2: Assign variable to dataset
features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions"))
activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))
subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject")
x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)
y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")
subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject")
x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions)
y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")

# Code below used in run_analysis.R function:

## 3: Merge data sets using rbind and cbind
x <- rbind(x_train, x_test)
y <- rbind(y_train, y_test)
subject <- rbind(subject_train, subject_test)
merged_data <- cbind(subject, y, x)
## 4: Extract mean and standard deviation for each measurment
tidy_data <- merged_data %>% select(subject, code, contains("mean"), contains("std"))
## 5: Assign descriptive activity names to names in the dataset
tidy_data$code <- activities[tidy_data$code, 2]
## 6: Label dataset with appropriate descriptive variable name
code column in tidy_data renamed into activities
All Acc in column’s name replaced by Accelerometer
All Gyro in column’s name replaced by Gyroscope
All BodyBody in column’s name replaced by Body
All Mag in column’s name replaced by Magnitude
All start with character f in column’s name replaced by Frequency
All start with character t in column’s name replaced by Time
## 7: Using data from from the tidy_data set (merged data), create a second data set (final_data) with the averages of each activity and each subject. Export final_data into a txt file
final_data <- tidy_data %>%
        group_by(subject, activity) %>%
        summarize_all(funs(mean))
write.table(final_data, "finaldata.txt", row.names = FALSE)
