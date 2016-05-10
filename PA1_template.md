---
title: "Personal Activity Analysis"
author: "Michel Voogd"
date: "10 May 2016"
output: html_document
---



### Introduction

This is an R Markdown document describing an analysis of personal activity data. For this analysis, data was captured by an activity monitoring device at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.

The data for this assignment can be downloaded from the [course website:](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip)

The variables included in this dataset are:  

* steps: Number of steps taking in a 5-minute interval (missing values are coded as NA)  
* date: The date on which the measurement was taken in YYYY-MM-DD format  
* interval: Identifier for the 5-minute interval in which measurement was taken  

The dataset is stored in a comma-separated-value (CSV) file and there are a total of 17,568 observations in this dataset.

### Objectives

In this analysis we will examine and visualize severeal statistics that describe the data set, such as the average number of steps per day, daily activity patterns and the difference between activity on weekdays versus weekends.

Outputs from the analysis are:  

1. Code for reading in the dataset and/or processing the data  
2. Histogram of the total number of steps taken each day  
3. Mean and median number of steps taken each day  
4. Time series plot of the average number of steps taken  
5. The interval that, on average, contains the maximum number of steps  
6. Code to describe and show a strategy for imputing missing data  
7. Histogram of the total number of steps taken each day after missing values are imputed  
8. Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends  
9. All of the R code needed to reproduce the results (numbers, plots, etc.) in the report (also included with output 1 to 8)  

### Code for preparing the workspace
For this analysis we use the ggplot2, Hmisc and dplyr packages. (Loading messages are suppressed here.)


```r
# Prep workspace ----------------------------------------------------------
library(ggplot2)
library(Hmisc)
library(dplyr)
```

### 1 - Code for reading in the dataset and/or processing the data
We assume the data file is in the same folder as this Markdown file, that that is also the working directory and that the file is called "data_activity.csv".


```r
# Loading and preprocessing the data --------------------------------------
Data <- read.csv("data_activity.csv", header = TRUE, sep = ",")
str(Data)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

### 2 - Histogram of the total number of steps taken each day
For this part of the analysis, we ignore missing values.

2.1 - Calculate the total number of steps taken per day 


```r
DataStepsByDate <- aggregate(data = Data, steps ~ date, sum)
head(DataStepsByDate, 3)
```

```
##         date steps
## 1 2012-10-02   126
## 2 2012-10-03 11352
## 3 2012-10-04 12116
```
  
2.2 - Make a histogram of the total number of steps taken each day

```r
par(cex = 0.75)
hist(DataStepsByDate$steps, col = "ivory2")
```

![plot of chunk histstepsbyday](figure/histstepsbyday-1.png)

### 3 - Mean and median number of steps taken each day

```r
Mean1 <- DataStepsByDate <- aggregate(data = Data, steps ~ date, mean)
Mean1
```

```
##          date      steps
## 1  2012-10-02  0.4375000
## 2  2012-10-03 39.4166667
## 3  2012-10-04 42.0694444
## 4  2012-10-05 46.1597222
## 5  2012-10-06 53.5416667
## 6  2012-10-07 38.2465278
## 7  2012-10-09 44.4826389
## 8  2012-10-10 34.3750000
## 9  2012-10-11 35.7777778
## 10 2012-10-12 60.3541667
## 11 2012-10-13 43.1458333
## 12 2012-10-14 52.4236111
## 13 2012-10-15 35.2048611
## 14 2012-10-16 52.3750000
## 15 2012-10-17 46.7083333
## 16 2012-10-18 34.9166667
## 17 2012-10-19 41.0729167
## 18 2012-10-20 36.0937500
## 19 2012-10-21 30.6284722
## 20 2012-10-22 46.7361111
## 21 2012-10-23 30.9652778
## 22 2012-10-24 29.0104167
## 23 2012-10-25  8.6527778
## 24 2012-10-26 23.5347222
## 25 2012-10-27 35.1354167
## 26 2012-10-28 39.7847222
## 27 2012-10-29 17.4236111
## 28 2012-10-30 34.0937500
## 29 2012-10-31 53.5208333
## 30 2012-11-02 36.8055556
## 31 2012-11-03 36.7048611
## 32 2012-11-05 36.2465278
## 33 2012-11-06 28.9375000
## 34 2012-11-07 44.7326389
## 35 2012-11-08 11.1770833
## 36 2012-11-11 43.7777778
## 37 2012-11-12 37.3784722
## 38 2012-11-13 25.4722222
## 39 2012-11-15  0.1423611
## 40 2012-11-16 18.8923611
## 41 2012-11-17 49.7881944
## 42 2012-11-18 52.4652778
## 43 2012-11-19 30.6979167
## 44 2012-11-20 15.5277778
## 45 2012-11-21 44.3993056
## 46 2012-11-22 70.9270833
## 47 2012-11-23 73.5902778
## 48 2012-11-24 50.2708333
## 49 2012-11-25 41.0902778
## 50 2012-11-26 38.7569444
## 51 2012-11-27 47.3819444
## 52 2012-11-28 35.3576389
## 53 2012-11-29 24.4687500
```

```r
Median1 <- DataStepsByDate <- aggregate(data = Data, steps ~ date, median)
Median1
```

```
##          date steps
## 1  2012-10-02     0
## 2  2012-10-03     0
## 3  2012-10-04     0
## 4  2012-10-05     0
## 5  2012-10-06     0
## 6  2012-10-07     0
## 7  2012-10-09     0
## 8  2012-10-10     0
## 9  2012-10-11     0
## 10 2012-10-12     0
## 11 2012-10-13     0
## 12 2012-10-14     0
## 13 2012-10-15     0
## 14 2012-10-16     0
## 15 2012-10-17     0
## 16 2012-10-18     0
## 17 2012-10-19     0
## 18 2012-10-20     0
## 19 2012-10-21     0
## 20 2012-10-22     0
## 21 2012-10-23     0
## 22 2012-10-24     0
## 23 2012-10-25     0
## 24 2012-10-26     0
## 25 2012-10-27     0
## 26 2012-10-28     0
## 27 2012-10-29     0
## 28 2012-10-30     0
## 29 2012-10-31     0
## 30 2012-11-02     0
## 31 2012-11-03     0
## 32 2012-11-05     0
## 33 2012-11-06     0
## 34 2012-11-07     0
## 35 2012-11-08     0
## 36 2012-11-11     0
## 37 2012-11-12     0
## 38 2012-11-13     0
## 39 2012-11-15     0
## 40 2012-11-16     0
## 41 2012-11-17     0
## 42 2012-11-18     0
## 43 2012-11-19     0
## 44 2012-11-20     0
## 45 2012-11-21     0
## 46 2012-11-22     0
## 47 2012-11-23     0
## 48 2012-11-24     0
## 49 2012-11-25     0
## 50 2012-11-26     0
## 51 2012-11-27     0
## 52 2012-11-28     0
## 53 2012-11-29     0
```

### 4 - Time series plot of the average number of steps taken

4.1 - Aggregate the data into mean steps by interval 

```r
DataStepsByItv <- aggregate(data = Data, steps ~ interval, mean)
```

4.2 - Create the plot

```r
PlotActivity <- ggplot(data = DataStepsByItv, aes(x = interval, y = steps)) +
      geom_line(size = 1, color = "ivory4") +
      labs(title = "Mean steps by interval") +
      theme_minimal()
PlotActivity
```

![plot of chunk dailypatternplot](figure/dailypatternplot-1.png)


### 5 - The interval that, on average, contains the maximum number of steps

```r
MaxItv <- with(DataStepsByItv, interval[[which.max(steps)]])
MaxItv
```

```
## [1] 835
```

### 6 - Code to describe and show a strategy for imputing missing data

6.1 Calculate and report the total number of rows with NAs

```r
sum(is.na(Data$steps))
```

```
## [1] 2304
```

6.2 Create a new dataset with NA's imputed by *the mean for the interval.*
After unsuccessfully trying to do this with the impute function from the Hmisc package, I decided to solve this with a merge/which(is.na()) construction.   

* First step, we create a temporary data frame, which is a merge between the original data and the aggregated data containing the average steps by interval. The original column with the nr. of steps for each interval is now called steps.x, and there is an additional column that contains the average nr. of steps for each interval, called steps.y.  
* Second step, in the temporary data frame, we identify the rows in steps.x that contain NA, and replace them with the corresponding values from steps.y.  
* Third step, we create a new copy of the original data frame, and replace the steps column with the imputed column from the temporary data frame.  
* Fourth and fifth step, since the merge has changed the order of the rows, we also replace the date and interval columns with those of the temporary data frame.  
* We now have a clean new data frame with NA's imputed by the mean of the interval.


```r
DataTemp <- merge(Data, DataStepsByItv, by = "interval")
DataTemp$steps.x[which(is.na(DataTemp$steps.x))] <-
      DataTemp$steps.y[which(is.na(DataTemp$steps.x))]
DataNew <- Data
DataNew$steps <- DataTemp$steps.x
DataNew$date <- DataTemp$date
DataNew$interval <- DataTemp$interval
head(DataNew, 3)
```

```
##      steps       date interval
## 1 1.716981 2012-10-01        0
## 2 0.000000 2012-11-23        0
## 3 0.000000 2012-10-28        0
```

### 7 - Histogram of the total number of steps taken each day after missing values are imputed  

7.1 - Calculate the total number of steps taken per day 


```r
DataStepsByDate2 <- aggregate(data = DataNew, steps ~ date, sum)
head(DataStepsByDate2, 3)
```

```
##         date    steps
## 1 2012-10-01 10766.19
## 2 2012-10-02   126.00
## 3 2012-10-03 11352.00
```
  
7.2 - Make a histogram of the total number of steps taken each day

```r
par(cex = 0.75)
hist(DataStepsByDate2$steps, col = "ivory2")
```

![plot of chunk histstepsbyday2](figure/histstepsbyday2-1.png)

### 8 - Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends  

8.1 - To do this we first need to Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.

* First step, we create a new data frame
* Second step, we create a new column with the day of the week
* Third step, we substitute Saturdays and Sundays by "Weekend", and Mondays thru Fridays by "Weekday".
* Fourth step, we mutate from a character to a factor variable so that we can use it as facet in our plot.


```r
DataNewer <- DataNew
DataNewer$dow <- weekdays(as.POSIXct(DataNewer$date), abbreviate = TRUE)
DataNewer$dow <-
      gsub("(Sat|Sun)",
           "Weekend", 
           DataNewer$dow, 
           ignore.case = TRUE)
DataNewer$dow <-
      gsub("(Mon|Tue|Wed|Thu|Fri)",
           "Weekday",
           DataNewer$dow,
           ignore.case = TRUE)
DataNewer$dow <- as.factor(DataNewer$dow)
```

8.2 - Now we aggregate the new data and create the plot

```r
DataStepsByItv2 <- aggregate(data = DataNewer, steps ~ interval + dow, mean)
PlotActivity2 <- ggplot(data = DataStepsByItv2, aes(x = interval, y = steps)) +
      geom_line(size = 1, color = "ivory4") +
      labs(title = "Mean steps by interval") +
      facet_grid(dow ~ .) +
      theme_minimal()
PlotActivity2
```

![plot of chunk plotdow](figure/plotdow-1.png)

### 9 - 
To run this entire code at once, here is the whole script:

```r
# Prep workspace ----------------------------------------------------------

library(ggplot2)
library(Hmisc)
library(dplyr)


# Loading and preprocessing the data --------------------------------------
Data <- read.csv("data_activity.csv", header = TRUE, sep = ",")
str(Data)


# Steps per day -----------------------------------------------------------

# Calculate the total nr of steps taken each day
DataStepsByDate <- aggregate(data = Data, steps ~ date, sum)
DataStepsByDate

# Make a histogram of the total number of steps taken each day
par(cex = 0.75)
hist(DataStepsByDate$steps, col = "ivory2")

# What are the mean and median total number of steps taken per day?
Mean1 <- DataStepsByDate <- aggregate(data = Data, steps ~ date, mean)
Mean1
Median1 <- DataStepsByDate <- aggregate(data = Data, steps ~ date, median)
Median1


# Daily activity ----------------------------------------------------------

# What is the average daily activity pattern?
DataStepsByItv <- aggregate(data = Data, steps ~ interval, mean)
PlotActivity <- ggplot(data = DataStepsByItv, aes(x = interval, y = steps)) +
      geom_line(size = 1, color = "ivory4") +
      labs(title = "Mean steps by interval") +
      theme_minimal()
PlotActivity

# Which interval, on avg across all days contains the maximum number of steps?
MaxItv <- with(DataStepsByItv, interval[[which.max(steps)]])
MaxItv


# Imputing missing values -------------------------------------------------

# Calculate and report the total number of rows with NAs
sum(is.na(Data$steps))

# Create a new dataset with NA's imputed by the mean for the interval.
# After unsuccessfully trying to do this with one of the obvious but not so easy
# impute solutions (Hmisc, MICE, etc) I finally decided to solve this the 
# conventional way. Gosh I miss Excel.
DataTemp <- merge(Data, DataStepsByItv, by = "interval")
DataTemp$steps.x[which(is.na(DataTemp$steps.x))] <-
      DataTemp$steps.y[which(is.na(DataTemp$steps.x))]
DataNew <- Data
DataNew$steps <- DataTemp$steps.x
DataNew$date <- DataTemp$date
DataNew$interval <- DataTemp$interval
head(DataNew, 3)

# Calculate the total nr of steps taken each day
DataStepsByDate2 <- aggregate(data = DataNew, steps ~ date, sum)
head(DataStepsByDate2, 3)

# Make a histogram of the total number of steps taken each day
par(cex = 0.75)
hist(DataStepsByDate2$steps, col = "ivory2")

# What are the mean and median total number of steps taken per day?
Mean2 <- DataStepsByDate2 <- aggregate(data = DataNew, steps ~ date, mean)
Mean2
Median2 <- DataStepsByDate2 <- aggregate(data = DataNew, steps ~ date, median)
Median2


# Weekdays vs weekends ----------------------------------------------------

# Are there differences in activity patterns between weekdays and weekends?
# Create a new factor variable in the dataset with two levels - "weekday" and
# "weekend" indicating whether a given date is a weekday or weekend day.

DataNewer <- DataNew
DataNewer$dow <- weekdays(as.POSIXct(DataNewer$date), abbreviate = TRUE)
DataNewer$dow <-
      gsub("(Sat|Sun)",
           "Weekend", 
           DataNewer$dow, 
           ignore.case = TRUE)
DataNewer$dow <-
      gsub("(Mon|Tue|Wed|Thu|Fri)",
           "Weekday",
           DataNewer$dow,
           ignore.case = TRUE)
DataNewer$dow <- as.factor(DataNewer$dow)

# Make a panel with time series plots of the average number of steps taken per
# interval, averaged across all weekdays or weekend days.

DataStepsByItv2 <- aggregate(data = DataNewer, steps ~ interval + dow, mean)
PlotActivity2 <- ggplot(data = DataStepsByItv2, aes(x = interval, y = steps)) +
      geom_line(size = 1, color = "ivory4") +
      labs(title = "Mean steps by interval") +
      facet_grid(dow ~ .) +
      theme_minimal()
PlotActivity2


# End ---------------------------------------------------------------------
```

