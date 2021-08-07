Reproducible Research Course Project 1
Thato Michael Moroenyane
July 20, 2021
Introduction
It is now possible to collect a lage amount of data about personal movement using activity monitoring devices such as a Fitbit, Nike Fuelband, or Jawbone Up. These type of devices are part of the"quatified self"movement-a group of enthuasiasts who take measurements about themselves regularly to improve their health, to find patterns in their behaviour, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during during the months of October and November, 2012 and the number of steps taken in 5 minute interval each day.

Assignment
The assignment will be be described parts.You will need to write a report that answers the questions detailed below. Ultimately, you will need to complete the entire assignment in a single R markdown document that can be processed by knitr and be transformed into an HTML file.

Throuhout your report make sure you always include the code that you used to generate the output you present. When writting code chunks in the R markdown document, always include the echo=TRUE so that someone else will be able to read the the code. The assigment will be evaluated via peer assessment so it essential that your peer evaluators be able to review the code to your analysis.

For the plotting aspects of this assignment, feel free to use any plotting system in R(i.e. base, lattice, ggplot2)

Fork/clone the Github repository created for assigment. You will submit this assignment by pushing your completed files into your forked repository on Githhub. The assignment submission willl consists of the URL to your Github repository and the SHA-1 commit ID for repository state.

Questions to be answered to be:
1.What is mean total number of steps taken per day? 2.What is the anerage daily activity pattern? 3.Imputing missing values 4.Are there differences in activity between weekdays and weekends?

Loading and preporopcessing the data
activity<-read.csv("activity.csv")
head(activity)
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
2.Process/transform the data(if necessary)into a format suitable for your analysis

It is ready to go.

1.What is mean number of steps taken per day?
For this part of the assigment, you can ignore the missing values in the dataset. 1. Calculate the total number of steps taken per day

totalStepsByDay<-aggregate(steps~date, activity, sum)
If you do not understand the difference between a histogram and a barplot, reserach the difference between them. Make a histogram of the total number of steps taken each day
hist(totalStepsByDay$steps, xlab="Class of Total Number of Steps per day", ylab = "Number of Days", main = "Total Numbrer of Steps taken each day")
 3. calculate and report the mean and median of the total number of steps taken per day

 mean_raw<-mean(totalStepsByDay$steps)
 mean_raw
## [1] 10766.19
``` Mean number of steps taken per day is 1.076618910^(4)

median_raw<-median(totalStepsByDay$steps)
Median number of steps taken per day is 10765.

2.What is the average daily activity pattern?
Make a time series plot(i.e. type = "l")of the 5-minute interval(x-axis)and the average number of stepsntaken, average across all days(y-axis)
The average number of steps taken

averageStepsbyInterval<-aggregate(steps~interval, activity, mean)
Time series plot of the average number of steps taken:

with(averageStepsbyInterval, plot(interval, steps, types = "l"))
## Warning in plot.window(...): "types" is not a graphical parameter
## Warning in plot.xy(xy, type, ...): "types" is not a graphical parameter
## Warning in axis(side = side, at = at, labels = labels, ...): "types" is not a
## graphical parameter

## Warning in axis(side = side, at = at, labels = labels, ...): "types" is not a
## graphical parameter
## Warning in box(...): "types" is not a graphical parameter
## Warning in title(...): "types" is not a graphical parameter


Which 5-minutes interval, on average across all the days in the dataset, contains the maximum number of steps?
averageStepsbyInterval[which.max(averageStepsbyInterval[,2]),1]
## [1] 835
3.Imputing missing values
Note that there are a number of days/intervals where there are missing values9coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

1.Calculate and report the total number of missing values in the dataset(i,e the total number of rows with NAs)

missingIndex<-is.na(activity[,1])
There are 2304 missing values in the dataset.

Devise a strategy for filling in all of the missing values in the datasets. The strategy doe not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
I decided to fill in all of the missing values in the dataset by the mean number of steps per interval.

Finding the mean number of steps per interval:

m<-mean(averageStepsbyInterval$steps)
Create a new dataset that is equal to the original dataset but with the missing data filled in.
Imputing missing values with m=37.3825996

activity1<-activity
activity1[missingIndex,1]<-m
head(activity1)
##     steps       date interval
## 1 37.3826 2012-10-01        0
## 2 37.3826 2012-10-01        5
## 3 37.3826 2012-10-01       10
## 4 37.3826 2012-10-01       15
## 5 37.3826 2012-10-01       20
## 6 37.3826 2012-10-01       25
Make a histogram of the total number of steps taken each day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?
Finding the total number of steps each day after missing values are imputed and making histogram:

totalStepsByDay1<-aggregate(steps~date,activity1,sum)
hist(totalStepsByDay1$steps, xlab="Class of Total Number of Steps per day", ylab="Number of Days", main = "Number of Steps taken each day after missing values are imputed")


To calculate the mean and median total number of steps per day, wr first find total number of steps per day

totalStepsByDay1<-aggregate(steps~date, activity1, sum)
mean_afterImput<-mean(totalStepsByDay1$steps)
mean_afterImput
## [1] 10766.19
Mean number of steps taken per day is 1.076618910^(4)

median_afterImput<-median(totalStepsByDay1$steps)
median_afterImput
## [1] 10766.19
Median number of the steps taken per day is 1.076618910^(4).

Since i imputed the missing values by the mean number of steps are interval, there is no difference in mean before and after imputting that is not surprising. The median has changed a little bit.

4.Are there differences in activity patterns between weekdays and weekends?
For this part the weekdays()function may be of some help here. Use the dataset with the filled-in missing values for this part.

Create a new factor variable in the dataset with two levels-"weekday" and "weekend" indicating whether a given date is a weekday day.
activity1$date<-as.Date(activity1$date)
library(dplyr)
## Warning: package 'dplyr' was built under R version 4.0.5
## 
## Attaching package: 'dplyr'
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
activity2<-activity1%>%mutate(dayType= ifelse(weekdays(activity1$date)=="Saturday" | weekdays(activity1$date)=="Sunday", "Weekend", "Weekday"))
head(activity2)
##     steps       date interval dayType
## 1 37.3826 2012-10-01        0 Weekday
## 2 37.3826 2012-10-01        5 Weekday
## 3 37.3826 2012-10-01       10 Weekday
## 4 37.3826 2012-10-01       15 Weekday
## 5 37.3826 2012-10-01       20 Weekday
## 6 37.3826 2012-10-01       25 Weekday
Make a plot containing a time plot(i.e type = "1")of the 5-minute interval(x-axis)and the average number of steps taken, averaged across all weekday days or weekend days(y-axis). See the README of what this plot look like using simulated data.

averageStepByDayTypeAndInterval<-activity2%>%group_by(dayType, interval)%>%summarize(averageStepByDay=sum(steps))
## `summarise()` has grouped output by 'dayType'. You can override using the `.groups` argument.
head(averageStepByDayTypeAndInterval)
## # A tibble: 6 x 3
## # Groups:   dayType [1]
##   dayType interval averageStepByDay
##   <chr>      <int>            <dbl>
## 1 Weekday        0             315.
## 2 Weekday        5             242.
## 3 Weekday       10             231.
## 4 Weekday       15             232.
## 5 Weekday       20             228.
## 6 Weekday       25             283.
library(lattice)
with(averageStepByDayTypeAndInterval,xyplot(averageStepByDay~interval | dayType, type="l", main="Total Number of Steps with in Intervals by dayType", xlab="Daily Intervals", ylab="Average Number of Steps"))


```Reproducible Research Course Project 1
Thato Michael Moroenyane
July 20, 2021
Introduction
It is now possible to collect a lage amount of data about personal movement using activity monitoring devices such as a Fitbit, Nike Fuelband, or Jawbone Up. These type of devices are part of the"quatified self"movement-a group of enthuasiasts who take measurements about themselves regularly to improve their health, to find patterns in their behaviour, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during during the months of October and November, 2012 and the number of steps taken in 5 minute interval each day.

Assignment
The assignment will be be described parts.You will need to write a report that answers the questions detailed below. Ultimately, you will need to complete the entire assignment in a single R markdown document that can be processed by knitr and be transformed into an HTML file.

Throuhout your report make sure you always include the code that you used to generate the output you present. When writting code chunks in the R markdown document, always include the echo=TRUE so that someone else will be able to read the the code. The assigment will be evaluated via peer assessment so it essential that your peer evaluators be able to review the code to your analysis.

For the plotting aspects of this assignment, feel free to use any plotting system in R(i.e. base, lattice, ggplot2)

Fork/clone the Github repository created for assigment. You will submit this assignment by pushing your completed files into your forked repository on Githhub. The assignment submission willl consists of the URL to your Github repository and the SHA-1 commit ID for repository state.

Questions to be answered to be:
1.What is mean total number of steps taken per day? 2.What is the anerage daily activity pattern? 3.Imputing missing values 4.Are there differences in activity between weekdays and weekends?

Loading and preporopcessing the data
activity<-read.csv("activity.csv")
head(activity)
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
2.Process/transform the data(if necessary)into a format suitable for your analysis

It is ready to go.

1.What is mean number of steps taken per day?
For this part of the assigment, you can ignore the missing values in the dataset. 1. Calculate the total number of steps taken per day

totalStepsByDay<-aggregate(steps~date, activity, sum)
If you do not understand the difference between a histogram and a barplot, reserach the difference between them. Make a histogram of the total number of steps taken each day
hist(totalStepsByDay$steps, xlab="Class of Total Number of Steps per day", ylab = "Number of Days", main = "Total Numbrer of Steps taken each day")
 3. calculate and report the mean and median of the total number of steps taken per day

 mean_raw<-mean(totalStepsByDay$steps)
 mean_raw
## [1] 10766.19
``` Mean number of steps taken per day is 1.076618910^(4)

median_raw<-median(totalStepsByDay$steps)
Median number of steps taken per day is 10765.

2.What is the average daily activity pattern?
Make a time series plot(i.e. type = "l")of the 5-minute interval(x-axis)and the average number of stepsntaken, average across all days(y-axis)
The average number of steps taken

averageStepsbyInterval<-aggregate(steps~interval, activity, mean)
Time series plot of the average number of steps taken:

with(averageStepsbyInterval, plot(interval, steps, types = "l"))
## Warning in plot.window(...): "types" is not a graphical parameter
## Warning in plot.xy(xy, type, ...): "types" is not a graphical parameter
## Warning in axis(side = side, at = at, labels = labels, ...): "types" is not a
## graphical parameter

## Warning in axis(side = side, at = at, labels = labels, ...): "types" is not a
## graphical parameter
## Warning in box(...): "types" is not a graphical parameter
## Warning in title(...): "types" is not a graphical parameter


Which 5-minutes interval, on average across all the days in the dataset, contains the maximum number of steps?
averageStepsbyInterval[which.max(averageStepsbyInterval[,2]),1]
## [1] 835
3.Imputing missing values
Note that there are a number of days/intervals where there are missing values9coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

1.Calculate and report the total number of missing values in the dataset(i,e the total number of rows with NAs)

missingIndex<-is.na(activity[,1])
There are 2304 missing values in the dataset.

Devise a strategy for filling in all of the missing values in the datasets. The strategy doe not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
I decided to fill in all of the missing values in the dataset by the mean number of steps per interval.

Finding the mean number of steps per interval:

m<-mean(averageStepsbyInterval$steps)
Create a new dataset that is equal to the original dataset but with the missing data filled in.
Imputing missing values with m=37.3825996

activity1<-activity
activity1[missingIndex,1]<-m
head(activity1)
##     steps       date interval
## 1 37.3826 2012-10-01        0
## 2 37.3826 2012-10-01        5
## 3 37.3826 2012-10-01       10
## 4 37.3826 2012-10-01       15
## 5 37.3826 2012-10-01       20
## 6 37.3826 2012-10-01       25
Make a histogram of the total number of steps taken each day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?
Finding the total number of steps each day after missing values are imputed and making histogram:

totalStepsByDay1<-aggregate(steps~date,activity1,sum)
hist(totalStepsByDay1$steps, xlab="Class of Total Number of Steps per day", ylab="Number of Days", main = "Number of Steps taken each day after missing values are imputed")


To calculate the mean and median total number of steps per day, wr first find total number of steps per day

totalStepsByDay1<-aggregate(steps~date, activity1, sum)
mean_afterImput<-mean(totalStepsByDay1$steps)
mean_afterImput
## [1] 10766.19
Mean number of steps taken per day is 1.076618910^(4)

median_afterImput<-median(totalStepsByDay1$steps)
median_afterImput
## [1] 10766.19
Median number of the steps taken per day is 1.076618910^(4).

Since i imputed the missing values by the mean number of steps are interval, there is no difference in mean before and after imputting that is not surprising. The median has changed a little bit.

4.Are there differences in activity patterns between weekdays and weekends?
For this part the weekdays()function may be of some help here. Use the dataset with the filled-in missing values for this part.

Create a new factor variable in the dataset with two levels-"weekday" and "weekend" indicating whether a given date is a weekday day.
activity1$date<-as.Date(activity1$date)
library(dplyr)
## Warning: package 'dplyr' was built under R version 4.0.5
## 
## Attaching package: 'dplyr'
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
activity2<-activity1%>%mutate(dayType= ifelse(weekdays(activity1$date)=="Saturday" | weekdays(activity1$date)=="Sunday", "Weekend", "Weekday"))
head(activity2)
##     steps       date interval dayType
## 1 37.3826 2012-10-01        0 Weekday
## 2 37.3826 2012-10-01        5 Weekday
## 3 37.3826 2012-10-01       10 Weekday
## 4 37.3826 2012-10-01       15 Weekday
## 5 37.3826 2012-10-01       20 Weekday
## 6 37.3826 2012-10-01       25 Weekday
Make a plot containing a time plot(i.e type = "1")of the 5-minute interval(x-axis)and the average number of steps taken, averaged across all weekday days or weekend days(y-axis). See the README of what this plot look like using simulated data.

averageStepByDayTypeAndInterval<-activity2%>%group_by(dayType, interval)%>%summarize(averageStepByDay=sum(steps))
## `summarise()` has grouped output by 'dayType'. You can override using the `.groups` argument.
head(averageStepByDayTypeAndInterval)
## # A tibble: 6 x 3
## # Groups:   dayType [1]
##   dayType interval averageStepByDay
##   <chr>      <int>            <dbl>
## 1 Weekday        0             315.
## 2 Weekday        5             242.
## 3 Weekday       10             231.
## 4 Weekday       15             232.
## 5 Weekday       20             228.
## 6 Weekday       25             283.
library(lattice)
with(averageStepByDayTypeAndInterval,xyplot(averageStepByDay~interval | dayType, type="l", main="Total Number of Steps with in Intervals by dayType", xlab="Daily Intervals", ylab="Average Number of Steps"))


```Reproducible Research Course Project 1
Thato Michael Moroenyane
July 20, 2021
Introduction
It is now possible to collect a lage amount of data about personal movement using activity monitoring devices such as a Fitbit, Nike Fuelband, or Jawbone Up. These type of devices are part of the"quatified self"movement-a group of enthuasiasts who take measurements about themselves regularly to improve their health, to find patterns in their behaviour, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during during the months of October and November, 2012 and the number of steps taken in 5 minute interval each day.

Assignment
The assignment will be be described parts.You will need to write a report that answers the questions detailed below. Ultimately, you will need to complete the entire assignment in a single R markdown document that can be processed by knitr and be transformed into an HTML file.

Throuhout your report make sure you always include the code that you used to generate the output you present. When writting code chunks in the R markdown document, always include the echo=TRUE so that someone else will be able to read the the code. The assigment will be evaluated via peer assessment so it essential that your peer evaluators be able to review the code to your analysis.

For the plotting aspects of this assignment, feel free to use any plotting system in R(i.e. base, lattice, ggplot2)

Fork/clone the Github repository created for assigment. You will submit this assignment by pushing your completed files into your forked repository on Githhub. The assignment submission willl consists of the URL to your Github repository and the SHA-1 commit ID for repository state.

Questions to be answered to be:
1.What is mean total number of steps taken per day? 2.What is the anerage daily activity pattern? 3.Imputing missing values 4.Are there differences in activity between weekdays and weekends?

Loading and preporopcessing the data
activity<-read.csv("activity.csv")
head(activity)
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
2.Process/transform the data(if necessary)into a format suitable for your analysis

It is ready to go.

1.What is mean number of steps taken per day?
For this part of the assigment, you can ignore the missing values in the dataset. 1. Calculate the total number of steps taken per day

totalStepsByDay<-aggregate(steps~date, activity, sum)
If you do not understand the difference between a histogram and a barplot, reserach the difference between them. Make a histogram of the total number of steps taken each day
hist(totalStepsByDay$steps, xlab="Class of Total Number of Steps per day", ylab = "Number of Days", main = "Total Numbrer of Steps taken each day")
 3. calculate and report the mean and median of the total number of steps taken per day

 mean_raw<-mean(totalStepsByDay$steps)
 mean_raw
## [1] 10766.19
``` Mean number of steps taken per day is 1.076618910^(4)

median_raw<-median(totalStepsByDay$steps)
Median number of steps taken per day is 10765.

2.What is the average daily activity pattern?
Make a time series plot(i.e. type = "l")of the 5-minute interval(x-axis)and the average number of stepsntaken, average across all days(y-axis)
The average number of steps taken

averageStepsbyInterval<-aggregate(steps~interval, activity, mean)
Time series plot of the average number of steps taken:

with(averageStepsbyInterval, plot(interval, steps, types = "l"))
## Warning in plot.window(...): "types" is not a graphical parameter
## Warning in plot.xy(xy, type, ...): "types" is not a graphical parameter
## Warning in axis(side = side, at = at, labels = labels, ...): "types" is not a
## graphical parameter

## Warning in axis(side = side, at = at, labels = labels, ...): "types" is not a
## graphical parameter
## Warning in box(...): "types" is not a graphical parameter
## Warning in title(...): "types" is not a graphical parameter


Which 5-minutes interval, on average across all the days in the dataset, contains the maximum number of steps?
averageStepsbyInterval[which.max(averageStepsbyInterval[,2]),1]
## [1] 835
3.Imputing missing values
Note that there are a number of days/intervals where there are missing values9coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

1.Calculate and report the total number of missing values in the dataset(i,e the total number of rows with NAs)

missingIndex<-is.na(activity[,1])
There are 2304 missing values in the dataset.

Devise a strategy for filling in all of the missing values in the datasets. The strategy doe not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
I decided to fill in all of the missing values in the dataset by the mean number of steps per interval.

Finding the mean number of steps per interval:

m<-mean(averageStepsbyInterval$steps)
Create a new dataset that is equal to the original dataset but with the missing data filled in.
Imputing missing values with m=37.3825996

activity1<-activity
activity1[missingIndex,1]<-m
head(activity1)
##     steps       date interval
## 1 37.3826 2012-10-01        0
## 2 37.3826 2012-10-01        5
## 3 37.3826 2012-10-01       10
## 4 37.3826 2012-10-01       15
## 5 37.3826 2012-10-01       20
## 6 37.3826 2012-10-01       25
Make a histogram of the total number of steps taken each day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?
Finding the total number of steps each day after missing values are imputed and making histogram:

totalStepsByDay1<-aggregate(steps~date,activity1,sum)
hist(totalStepsByDay1$steps, xlab="Class of Total Number of Steps per day", ylab="Number of Days", main = "Number of Steps taken each day after missing values are imputed")


To calculate the mean and median total number of steps per day, wr first find total number of steps per day

totalStepsByDay1<-aggregate(steps~date, activity1, sum)
mean_afterImput<-mean(totalStepsByDay1$steps)
mean_afterImput
## [1] 10766.19
Mean number of steps taken per day is 1.076618910^(4)

median_afterImput<-median(totalStepsByDay1$steps)
median_afterImput
## [1] 10766.19
Median number of the steps taken per day is 1.076618910^(4).

Since i imputed the missing values by the mean number of steps are interval, there is no difference in mean before and after imputting that is not surprising. The median has changed a little bit.

4.Are there differences in activity patterns between weekdays and weekends?
For this part the weekdays()function may be of some help here. Use the dataset with the filled-in missing values for this part.

Create a new factor variable in the dataset with two levels-"weekday" and "weekend" indicating whether a given date is a weekday day.
activity1$date<-as.Date(activity1$date)
library(dplyr)
## Warning: package 'dplyr' was built under R version 4.0.5
## 
## Attaching package: 'dplyr'
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
activity2<-activity1%>%mutate(dayType= ifelse(weekdays(activity1$date)=="Saturday" | weekdays(activity1$date)=="Sunday", "Weekend", "Weekday"))
head(activity2)
##     steps       date interval dayType
## 1 37.3826 2012-10-01        0 Weekday
## 2 37.3826 2012-10-01        5 Weekday
## 3 37.3826 2012-10-01       10 Weekday
## 4 37.3826 2012-10-01       15 Weekday
## 5 37.3826 2012-10-01       20 Weekday
## 6 37.3826 2012-10-01       25 Weekday
Make a plot containing a time plot(i.e type = "1")of the 5-minute interval(x-axis)and the average number of steps taken, averaged across all weekday days or weekend days(y-axis). See the README of what this plot look like using simulated data.

averageStepByDayTypeAndInterval<-activity2%>%group_by(dayType, interval)%>%summarize(averageStepByDay=sum(steps))
## `summarise()` has grouped output by 'dayType'. You can override using the `.groups` argument.
head(averageStepByDayTypeAndInterval)
## # A tibble: 6 x 3
## # Groups:   dayType [1]
##   dayType interval averageStepByDay
##   <chr>      <int>            <dbl>
## 1 Weekday        0             315.
## 2 Weekday        5             242.
## 3 Weekday       10             231.
## 4 Weekday       15             232.
## 5 Weekday       20             228.
## 6 Weekday       25             283.
library(lattice)
with(averageStepByDayTypeAndInterval,xyplot(averageStepByDay~interval | dayType, type="l", main="Total Number of Steps with in Intervals by dayType", xlab="Daily Intervals", ylab="Average Number of Steps"))


```
