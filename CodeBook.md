Code Book
=========


I. Description of the tidy data set
-----------------------------------

The data set contained in the file "tidyset.txt" contains column names
and row numbers in addition to the data in the data frame.
The row numbers are simply successive numbers.
The names of the columns 3 to 68 are taken from the "features.txt" file
in the raw data set and are described in more detail in the raw data set's
README.txt and features_info.txt files.
The tidy data set contains only a subset of the features described there,
namely, those variables that contain the mean values and standard deviations
for each signal type, i.e. the mean() and std() variables for all 33 signals,
as described in features_info.txt

Column 1 of the table identifies the experimental subject for which the
measurements in that row were taken by their id number (there were 30 subjects,
numbered successively from 1 to 30).
Column 2 contains the activity carried out by the subject during which the
measurements in that row were taken. 6 different activities were distinguished:
"walk", "walk upstairs", "walk downstairs", "sit", "stand", "lie".

This table has 180 rows (6 activities * 30 subjects = 180)
and 68 columns (2 for subject and activity + 33 means + 33 stds = 68).



II. Transformations carried out to create the tidy data set from the raw set
----------------------------------------------------------------------------

1. First the files from the training data set were loaded into data frames.
The subject and activity columns (containing the id number of each subject
and the id number of each activity, respectively) were loaded from the
train/subject_train.txt and the train/y_train.txt files into two separate,
one-column dataframes (train.subj and train.activity) as factor type values.
Then the measurement data for the 561 variables (as described in the raw
data set file "features.txt") were loaded into a third table from the file
train/X_train.txt; the latter data frame is called train.data.
When reading the data for this latter table the parameter fill=TRUE seems to
be required, otherwise the read process fails with an error message that
claims that line 2 of the input file is not the same length as the first line.
The parameter fill=TRUE is supposed to fix this by inserting NA values for missing
data when the rows do not contain the same number of columns, but in fact once
this parameter is specified the data set loads correctly and there are no NA
values in the table after all. So I don't know how fill=TRUE solves the previous
problem, but since the read.table works this way and it does not seem to cause
any other problems, I see no reason not to use it.

Note: the files in the "Inertial Signals" directory of the raw data set
were not required for this project and were not loaded.

2. The list of activities in the data frame train.activity are present as
number codes in the raw data.
In order to transform these into descriptive labels these numbers had to be
unlisted into a factor, the levels of the factor (1 to 6) renamed to the labels
as described in the "activity_labels.txt" file in the raw data set, and the
resulting labeled factor was converted back into a table column in the original
data frame train.activity

Note: this labeling step could have been carried out later as well, after
combining the train and test data.

3. The data frames train.subj, train.activity and train.data are merged into
a single data frame and stored as test.data.
train.subj is the first, train.activity the second, and the original train.data
the third to 563rd columns of this data set.

4. Steps 1 through 3 are repeated for the files in the test data set.
The following files are loaded into the following data frames:
"test/subject_test.txt" => test.subj
"test/y_test.txt" => test.activity
"test/X_test.txt" => test.data
These are then merged as described in test.data

5. The rows in the train.data and the test.data data frames are combined in a
single table called all.data using rbind.

6. The data in "features.txt" are read into a data frame (omitting the row
numbers), unlisted and transformed into a character vector of length 561,
which contains the descriptive labels for columns 3 to 563 of all.data.
The columns are then labeled using this character vector. The first and second
column are labeled as "subject" and "activity".

7. First those columns the names of which contain the substring "mean(" or "std"
are extracted from all.data into a separate table called means.and.stds,
then the "subject" and "activity" columns are also added to the new table.

8. Using the aggregate function a new data frame is created that contains
the means for the values of each of the 66 mean and std variables, grouped
firstly by subject and secondly by activity.
Since the aggregate function switches the order of these two columns,
they are put in their original column in the next step.
Since the aggregate function also renames the two grouping columns to
"Group.1" and "Group.2", the first two columns are then also renamed
back to their original more descriptive names "subject" and "activity".

This completes the description of the steps taken to transform the data set.