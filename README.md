# Getting-and-Cleaning-Data-Course-Project

####Load and process data.
xtest <- read.table("C:/Users/faheera/Desktop/Coursera/course_3/Week_4_Assignment/test/X_test.txt")
ytest <- read.table("C:/Users/faheera/Desktop/Coursera/course_3/Week_4_Assignment/test/Y_test.txt")
subtest <- read.table("C:/Users/faheera/Desktop/Coursera/course_3/Week_4_Assignment/test/subject_test.txt")
xtrain <- read.table("C:/Users/faheera/Desktop/Coursera/course_3/Week_4_Assignment/train/X_train.txt")
ytrain <- read.table("C:/Users/faheera/Desktop/Coursera/course_3/Week_4_Assignment/train/Y_train.txt")

features <- read.table("C:/Users/faheera/Desktop/Coursera/course_3/Week_4_Assignment/features.txt")
features1 <-read.table("C:/Users/faheera/Desktop/Coursera/course_3/Week_4_Assignment/features.txt")[,2]

# Load activity data

ytrain[,2] = activity_labels[ytrain[,1]]

names(ytrain) = c("Activity_ID", "Activity_Label")


##Merge data
test_data <- cbind(ytest, xtest)
train_data <- cbind(ytrain, xtrain)
data = rbind(test_data, train_data)
####Extracts only the measurements on the mean and standard deviation for each measurement.
extract_features <- grepl("mean|std", features1)
X_test = xtest[,extract_features]

labels_1   = c("subject", "Activity_ID", "Activity_Label")

data_labels = setdiff(colnames(data), labels_1)

melt_data = melt.data.table(data, id = labels_1, measure.vars = data_labels)

# Apply mean function to dataset using dcast function

tidydata   = dcast(melt_data, Activity_Label ~ variable, mean)


write.table(tidy_data, file = "./tidy_data.txt")
