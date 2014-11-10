# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data


```r
unzip("activity.zip")
data<-read.csv("activity.csv",header = TRUE)
```


## What is mean total number of steps taken per day?


```r
#Make a histogram of the total number of steps taken each day
library(data.table)
```

```
## Warning: package 'data.table' was built under R version 3.1.2
```

```r
activity <- data.table(data)
activity.totalSteps<-activity[,sum(steps,na.rm = TRUE),by=date]
hist(activity.totalSteps$V1,breaks=10,main="Histogram of total steps per day over two months",
     xlab="Total steps per day")
```

![](./PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
#Calculate and report the mean and median total number of steps taken per day
activity.mean=mean(activity.totalSteps$V1,na.rm = TRUE)
activity.median = median(activity.totalSteps$V1,na.rm = TRUE)
```
mean =  9354.2295082

median = 10395


## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
