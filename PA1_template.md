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

activity.imputed<-activity
activity.meanByDay<-activity[,mean(steps,na.rm = TRUE),by=date]
activity.meanByDay[is.na(activity.meanByDay)]<-0

for(i in 1:length(activity$steps)){
  if(bad[i]){ 
    activity.imputed$steps[i]<-activity.meanByDay[date == activity.imputed$date[i]]$V1
    }
  }

activity.imputed.totalSteps<-activity.imputed[,sum(steps,na.rm = TRUE),by=date]
hist(activity.imputed.totalSteps$V1,breaks=10,main="Histogram of total steps per day over two months",
     xlab="Total steps per day")
```

![](./PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

```r
activity.imputed.mean=mean(activity.imputed.totalSteps$V1,na.rm = TRUE)
activity.imputed.median = median(activity.imputed.totalSteps$V1,na.rm = TRUE)
```
Total number of rows with NAs: 2304

mean.imputed =  9354.2295082

median.imputed = 1.0395\times 10^{4}



## Are there differences in activity patterns between weekdays and weekends?
