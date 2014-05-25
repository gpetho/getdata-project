getdata-project
===============

Coursera course project

This script creates the tidy dataset according to the
instructions of the Coursera getdata-003 course.

The "stats" R package is required for it to work.
The working directory must be set to the "UCI HAR Dataset"
directory containing the project data.
Simply run the script. It will produce the file
"tidyset.txt", which is the tidy data set required.

This data set can be loaded simply with
df <- read.table("tidyset.txt")

