---
title: "Project week 2"
author: "You"
date: "15/06/2020"
output: 
  html_document: 
    keep_md: yes

---



## Loading and Preprocessing the data
  
  The data is loaded and reprocessed in R using the following code. The t5 
variable is used to store the intervals. This varable will be used in the time
series plots
  

```r
data<-read.csv("activity.csv" )
data$date<-as.Date(as.character(data$date),"%Y-%m-%d")
t5<-unique(data$interval)
```

## What is mean total number of steps taken per day?



```r
s<-split(data,data$date)
s1<-lapply(s,function(x) { sum(x[,1],na.rm = TRUE) })
s2<-as.numeric(s1)
hist(s2,xlab = "total number of Steps",main="Histogram of the total number of steps taken each day")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
mean1<-mean(s2)
median1<-median(s2)
```

The mean and median of the total number of steps taken each day is 9354.2295082 and
1.0395\times 10^{4} respectively.

## What is the average daily activity pattern?


```r
s5<-split(data,data$interval)
s6<-lapply(s5,function(x) { mean(x[,1],na.rm = TRUE) })
s7<-as.numeric(s6)
plot(t5,s7,type = "l",xlab="Interval",ylab = "Number of Steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```r
s8<-cbind(t5,s7)
s8<-as.data.frame(s8)
max(s7)
```

```
## [1] 206.1698
```

```r
maxinterval<-s8[max(s8$s7),1]
```
The 5-minute interval, on average across all the days in the dataset which
contains the maximum number of steps is 1705.

## Imputing missing values

The total number of missing values in the dataset is given below.

```r
t6<-colSums(is.na(data)) 
t6
```

```
##    steps     date interval 
##     2304        0        0
```

```r
impute<-function(x)
{
       p3<- mean(x[,1],na.rm = TRUE) 
        p2<-is.na(x[,1])
        p4<-which(p2)
        x[p4,1]<-p3
        x
}
p1<-lapply(s5,impute)
library(dplyr)
p7<-bind_rows(p1)
p7<-p7[order(p7$date),]
a<-split(p7,p7$date)
a1<-lapply(a,function(x) { sum(x[,1],na.rm = TRUE) })
a2<-as.numeric(a1)
hist(a2,xlab = "total number of Steps",main="Histogram of the total number of steps taken each day")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
mean2<-mean(a2)
median2<-median(a2)
```
The mean for that 5-minute interval, used for filling the missing values in the dataset.The mean and median of the total number of steps taken each day is 
1.0766189\times 10^{4} and 1.0766189\times 10^{4} respectively. 
There is an increase in the number of days with number of steps between 10000
and 15000 and a decrease in the number of days with number of steps between 0
and 5000.

## Are there differences in activity patterns between weekdays and weekends?


```r
p7$day<-weekdays(p7$date)
r1<-split(p7,p7$day)
r1$Monday$day<-"weekday"
r1$Tuesday$day<-"weekday"
r1$Wednesday$day<-"weekday"
r1$Thursday$day<-"weekday"
r1$Friday$day<-"weekday"
r1$Saturday$day<-"weekend"
r1$Sunday$day<-"weekend"
r2<-bind_rows(r1)
r2$day<-as.factor(r2$day)       
r3<-r2[r2$day=="weekday",]
r4<-r2[r2$day=="weekend",]
##What is the average daily activity pattern for weekday
r5<-split(r3,r3$interval)
r6<-lapply(r5,function(x) { mean(x[,1],na.rm = TRUE) })
r7<-as.numeric(r6)
##What is the average daily activity pattern for weekend
r8<-split(r4,r4$interval)
r9<-lapply(r8,function(x) { mean(x[,1],na.rm = TRUE) })
r10<-r7<-as.numeric(r9)
par(mfrow=c(2,1),mar=c(4,4,4,1))
plot(t5,r10,type = "l",xlab = "Interval",ylab = "Number of Steps",main="weekend")
plot(t5,r7,type = "l",xlab = "Interval",ylab = "Number of Steps",main="weekday")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

There is no difference between the activity pattern during the weekday and 
during the weekend.
