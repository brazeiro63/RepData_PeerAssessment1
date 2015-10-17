---
title: "Reproducible Research: Peer Assessment 1"
output: 
html_document:
keep_md: true
---

## Loading and preprocessing the data
To meet the requested this task, you must start loading the data, cleaning them and formatting them properly.


```r
install.packages("Cron")
```

```
## Installing package into 'C:/Users/PauloRicardo/Documents/R/win-library/3.2'
## (as 'lib' is unspecified)
```

```r
library(chron)
library(dplyr)
## Decompress the zip file to the same path
unzip(zipfile = "activity.zip")

## Read the csv file and store it in a data.frame while sort the variables
## in a specific order
fdata <- select(tbl_df(read.csv("activity.csv")),date, interval, steps)
```

## What is mean total number of steps taken per day?
This question requires an aggregation of the data in terms of steps per day. To do this you need to add up all the steps a day.


```r
steps_by_day <- aggregate(steps ~ date, fdata, sum)
summary(steps_by_day)
```

```
##          date        steps      
##  2012-10-02: 1   Min.   :   41  
##  2012-10-03: 1   1st Qu.: 8841  
##  2012-10-04: 1   Median :10765  
##  2012-10-05: 1   Mean   :10766  
##  2012-10-06: 1   3rd Qu.:13294  
##  2012-10-07: 1   Max.   :21194  
##  (Other)   :47
```

At this point it is possible to check the frequency distribution of the number of steps by means of a histogram.

```r
hist(steps_by_day$steps, 
     main = "Steps distribution",
     ylab = "frequency",
     xlab = "steps")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 

Finnaly one needs to calculate the mean and median number of steps per day.

```r
mean_steps <- mean(steps_by_day$steps, na.rm = TRUE)
medi_steps <- median(steps_by_day$steps, na.rm = TRUE)
```

The result follows:  
**Mean** of steps taken by day is **10766.19**   
**Median** of steps taken by day is **10765**.  

----

## What is the average daily activity pattern?


```r
maxi_steps_by_day <- aggregate(steps ~ date, fdata, max )
with(maxi_steps_by_day, plot(date,steps,type = "l" ))
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png) 

```r
mean_steps_by_interval <- aggregate(steps ~ interval, fdata, mean )
with(mean_steps_by_interval, plot(interval,steps,type = "l" ))
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-2.png) 

## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?

```r
## Add a column with the corresponding type of day (weekday or weekend).
fdata <- mutate(fdata, 
                day = factor(is.weekend(date), 
                             c(TRUE, FALSE), 
                             c("weekend", "weekday")))
```
