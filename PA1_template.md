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
activity.totalSteps<-activity[,mean(steps,na.rm = TRUE),by=date]
hist(activity.totalSteps$V1,breaks=10,main="Histogram of total steps per day over two months",
     xlab="Total steps per day")
```

![](./PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
#Calculate and report the mean and median total number of steps taken per day

activity.mean=mean(activity.totalSteps$V1,na.rm = TRUE)
activity.median = median(activity.totalSteps$V1,na.rm = TRUE)
```
mean =  37.3825996

median = 37.3784722


## What is the average daily activity pattern?


```r
#Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

activity.totalStepsInterval <- activity[,mean(steps,na.rm = TRUE),by=interval]
plot(activity.totalStepsInterval$interval,activity.totalStepsInterval$V1, type = 'l',xlab='Time Interval of 5 Min',ylab="Average number of steps taken")
```

![](./PA1_template_files/figure-html/unnamed-chunk-3-1.png) 



```r
#Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

maxSteps<-activity.totalStepsInterval[which.max(activity.totalStepsInterval$V1)]
```

Maximum number of steps: 206.1698113

Time Interval for maximum number of steps: 835



## Imputing missing values

```r
bad<-is.na(activity$steps)
totalNA<-sum(bad)
```
Total number of rows with NAs: 2304


## Are there differences in activity patterns between weekdays and weekends?
