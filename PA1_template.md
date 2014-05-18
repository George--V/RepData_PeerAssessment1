# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

1. [Download the data from this location]
(https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip)
2. Unzip the file and move the csv to the directory to be used as working directory.
3. Set the working directory in R with setwd()
4. read the data into a data frame:


```r
DF <- read.csv("activity.csv")
```


## What is mean total number of steps taken per day?

1. Histogram of steps groupped by date (omitting NAs):


```r
x <- aggregate(steps ~ date, DF, FUN = sum)
hist(x$steps)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 


2. Mean and median of steps per day


```r
meanSteps <- mean(x$steps)
medianSteps <- median(x$steps)
```


The mean steps per day is 10766

The median steps per day is 10765

## What is the average daily activity pattern?

1. Plot average steps taken by interval 


```r
x1 <- aggregate(steps ~ interval, DF, FUN = mean)
plot(x1$interval, x1$steps, type = "l")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 


2. Which 5-minute interval contains the maximum number of steps


```r
maxSteps <- x1$interval[which.max(x1$steps)]
```


The maximum number of steps(on average) is at interval 835

## Imputing missing values

There are a number of missing values (NA) in the original data, in particular
these NAs affect steps:

```r
naSteps <- sum(is.na(DF$steps))
naDate <- sum(is.na(DF$date))
naInterval <- sum(is.na(DF$interval))
```


The number of missing entries in steps is 2304.

The number of missing entries in dates is 0.

The number of missing entries in interval is 0.

### Imputation strategy: use average steps to populate missing values


```r
indx <- which(is.na(DF[, 1]) == TRUE)
DF[indx, 1] <- mean(x1$steps[x1$interval == DF[indx, 3]], na.rm = TRUE)
```


Histogram of steps groupped by date (After imputing):


```r
x <- aggregate(steps ~ date, DF, FUN = sum)
hist(x$steps)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8.png) 


Mean and median of steps per day


```r
meanSteps2 <- mean(x$steps)
medianSteps2 <- median(x$steps)
```


The mean steps per day is 10766

The median steps per day is 10766

## Are there differences in activity patterns between weekdays and weekends?