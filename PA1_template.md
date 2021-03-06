NikeFuel Steps Data -- by Iñigo López
========================================================
- The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day. "activity.csv"


## Loading and preprocessing the data:

I'm going to read the data, in activity.csv file, assigning NA to missing values, and save in a rowStepData variable:

```r
rowStepData <- read.csv("./activity.csv", header = TRUE, na.strings = "NA")
```


I'm going to see if the file has been well read, checking the first values

```r
head(rowStepData)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```


I'm going to see what kind of values contains the variable 'date':

```r
class(rowStepData$date)
```

```
## [1] "factor"
```


Ok. The variable 'date' has been read as a Factor. I need to convert into Date class:

```r
rowStepData$date <- as.Date(rowStepData$date, format = "%Y-%m-%d")
```


I'm going to check now...

```r
class(rowStepData$date)
```

```
## [1] "Date"
```

It's OK!!

How many missing value are there in the dataset?

```r
sum(is.na(rowStepData$step))
```

```
## [1] 2304
```


Now, i'm going to created a new dataset removing all missing values:

```r
ignored_NA_StepData <- rowStepData[!is.na(rowStepData$step), ]
```

...and going to see if the file has been well read, checking the first values

```r
head(ignored_NA_StepData)
```

```
##     steps       date interval
## 289     0 2012-10-02        0
## 290     0 2012-10-02        5
## 291     0 2012-10-02       10
## 292     0 2012-10-02       15
## 293     0 2012-10-02       20
## 294     0 2012-10-02       25
```



## What is mean total number of steps taken per day?

I'm going to calculate the mean of steps per day, ignoring missing values values, so i'm going to used "ignored_NA_StepData" dataset:
- First i'm going to split the dataset into diferent days:

```r
splitDataByDate <- split(ignored_NA_StepData$steps, ignored_NA_StepData$date)
```

- 2th, i'm going to create a vector with all days in dataset:

```r
allDays <- as.Date(sort(unique(ignored_NA_StepData$date)), format = "%Y-%m-%d")
```


- And now i'm going to calculate sum, mean and median of all step splitted by day:


```r
sumStepByDate <- as.data.frame(sapply(splitDataByDate, sum))
names(sumStepByDate) <- c("Steps")
mean(sumStepByDate$Steps)
```

```
## [1] 10766
```

```r
median(sumStepByDate$Steps)
```

```
## [1] 10765
```


I'm going generate a histogram with the frecuency of total sum of steps by day: 

```r
hist(sumStepByDate$Steps, main = "Total steps by day frecuency", xlab = "Number of steps by day", 
    ylab = "Number of days", col = "red", breaks = nrow(sumStepByDate))
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12.png) 

..and save it into '/figures/' path:

```r
png(filename = "figures/plot1.png", width = 480, height = 480, units = "px")
hist(sumStepByDate$Steps, main = "Total steps by day frecuency", xlab = "Number of steps by day", 
    ylab = "Number of days", col = "red", breaks = nrow(sumStepByDate))
dev.off()
```

```
## pdf 
##   2
```


Now, I'm going to create a file with the sum of steps day by day, the mean and the median of all days in dataset:


```r
plot(allDays, sumStepByDate$Steps, type = "l", ylab = "Sum of step", xlab = "Day")
lines(allDays, rep(median(sumStepByDate$Steps), length(allDays)), type = "l", 
    col = "blue")
lines(allDays, rep(mean(sumStepByDate$Steps), length(allDays)), type = "l", 
    col = "red")
legend("topright", c("Mean", "Median"), lty = 1, col = c("red", "blue"), cex = 0.95)
```

![plot of chunk unnamed-chunk-14](figure/unnamed-chunk-14.png) 



```r
png(filename = "figures/plot2.png", width = 480, height = 480, units = "px")
plot(allDays, sumStepByDate$Steps, type = "l", ylab = "Sum of step", xlab = "Day")
lines(allDays, rep(median(sumStepByDate$Steps), length(allDays)), type = "l", 
    col = "blue")
lines(allDays, rep(mean(sumStepByDate$Steps), length(allDays)), type = "l", 
    col = "red")
legend("topright", c("Mean", "Median"), lty = 1, col = c("red", "blue"), cex = 0.95)
dev.off()
```

```
## pdf 
##   2
```



Is posible the median line isn't see, because is similar value to mean:
Mean:

```r
mean(sumStepByDate$Steps)
```

```
## [1] 10766
```

Median:

```r
median(sumStepByDate$Steps)
```

```
## [1] 10765
```


# What is the average daily activity pattern?

now i'm going to make a Interval segmentation:

```r
allIntervals <- sort(unique(ignored_NA_StepData$interval))
splitDataByInterval <- split(ignored_NA_StepData$steps, ignored_NA_StepData$interval)
```


..and calculate the mean for each interval..

```r
meanStepByInterval <- as.data.frame(sapply(splitDataByInterval, mean))
names(meanStepByInterval) <- c("Steps")
```


Drawing the solution:

```r
plot(allIntervals, meanStepByInterval$Steps, type = "l", main = "5-Interval step mean", 
    xlab = "5-interval", ylab = "Mean of steps")
lines(allIntervals, rep(max(meanStepByInterval$Steps), length(allIntervals)), 
    type = "l", col = "red")
legend("topright", c("Max"), lty = 1, col = c("red"), cex = 0.95)
```

![plot of chunk unnamed-chunk-20](figure/unnamed-chunk-20.png) 



```r
png(filename = "figures/plot3.png", width = 480, height = 480, units = "px")
plot(allIntervals, meanStepByInterval$Steps, type = "l", main = "5-Interval step mean", 
    xlab = "5-interval", ylab = "Mean of steps")
lines(allIntervals, rep(max(meanStepByInterval$Steps), length(allIntervals)), 
    type = "l", col = "red")
legend("topright", c("Max"), lty = 1, col = c("red"), cex = 0.95)
dev.off()
```

```
## pdf 
##   2
```

       
Maxim mean of step in an <b>835</b> 5-minutes interval: 

```r
max(meanStepByInterval$Steps)
```

```
## [1] 206.2
```



# Imputing missing values

Now i going to complete the missing date in the raw data with simulated data.
Copy the rawData

```r
simStepData <- rowStepData
```


Calculate the list of means split by date:

```r
meanSByDate <- sapply(splitDataByDate, mean)
```


Fill in the NA values with the mean of the day:

```r
for (i in 1:nrow(simStepData)) {
    if (is.na(simStepData[i, 1])) {
        date_i <- simStepData[i, 2]
        simStepData[i, 1] <- meanSByDate[[as.factor(date_i)]]
    }
}
```


Now, with the new dataset, repeat the same to obtain the graphics:

```r
splitSimulatedDataByDate <- split(simStepData$steps, simStepData$date)

sumSimulaStepByDate <- as.data.frame(sapply(splitSimulatedDataByDate, sum))
names(sumSimulaStepByDate) <- c("Steps")
mean(sumSimulaStepByDate$Steps)
```

```
## [1] 9371
```

```r
median(sumSimulaStepByDate$Steps)
```

```
## [1] 10395
```

We can see that both the mean and the median decreased to fairly allocate data NA and put the average daily data. This is due to the large number of 0's that there now that make the mean and median decrease.

Drawing the solution:

```r
hist(sumSimulaStepByDate$Steps, main = "Total steps by day frecuency", xlab = "Number of steps by day", 
    ylab = "Number of days", col = "red", breaks = nrow(sumSimulaStepByDate))
```

![plot of chunk unnamed-chunk-27](figure/unnamed-chunk-27.png) 



```r
png(filename = "figures/plot4.png", width = 480, height = 480, units = "px")
hist(sumSimulaStepByDate$Steps, main = "Total steps by day frecuency", xlab = "Number of steps by day", 
    ylab = "Number of days", col = "red", breaks = nrow(sumSimulaStepByDate))
dev.off()
```

```
## pdf 
##   2
```

 
 

```r
allSimDays <- as.Date(sort(unique(simStepData$date)), format = "%Y-%m-%d")
```



```r
plot(allSimDays, sumSimulaStepByDate$Steps, type = "l", ylab = "Sum of step", 
    xlab = "Day")
lines(allSimDays, rep(median(sumSimulaStepByDate$Steps), length(allSimDays)), 
    type = "l", col = "blue")
lines(allSimDays, rep(mean(sumSimulaStepByDate$Steps), length(allSimDays)), 
    type = "l", col = "red")
legend("topright", c("Mean", "Median"), lty = 1, col = c("red", "blue"), cex = 0.95)
```

![plot of chunk unnamed-chunk-30](figure/unnamed-chunk-30.png) 



```r
png(filename = "figures/plot5.png", width = 480, height = 480, units = "px")
plot(allSimDays, sumSimulaStepByDate$Steps, type = "l", ylab = "Sum of step", 
    xlab = "Day")
lines(allSimDays, rep(median(sumSimulaStepByDate$Steps), length(allSimDays)), 
    type = "l", col = "blue")
lines(allSimDays, rep(mean(sumSimulaStepByDate$Steps), length(allSimDays)), 
    type = "l", col = "red")
legend("topright", c("Mean", "Median"), lty = 1, col = c("red", "blue"), cex = 0.95)
dev.off()
```

```
## pdf 
##   2
```


# Are there differences in activity patterns between weekdays and weekends?
 

I'm going to create a new variable in dataset called 'dateWeek' and put the value of day of the week (weekdays function)

```r
simStepData$dateWeek <- weekdays(simStepData$date)
```

..and now i'm going to compare if dateWeek is 'saturday' or 'sunday' ( 'sábado' ó 'domingo' in my Spanish version of OS) and put the correct factor ('Weekend or Weekday') depend on it.

```r
for (i in 1:nrow(simStepData)) {
    if (simStepData[i, 4] == "domingo" | simStepData[i, 4] == "sábado") {
        simStepData[i, 4] <- "Weekend"
    } else {
        simStepData[i, 4] <- "Weekday"
    }
}
```


Now i'm going to split the dataset by dateWeek and then separate inte two diferent datasets:

```r
weekDaySplit <- split(simStepData, simStepData$dateWeek)
weekendData <- weekDaySplit[["Weekend"]]
weekdayData <- weekDaySplit[["Weekday"]]
```



## First Method: basic system graph

i'm going to create an other split in both datasets by Interval


```r
allweekendIntervals <- sort(unique(weekendData$interval))
splitweekendDataByInterval <- split(weekendData$steps, weekendData$interval)

allweekdayIntervals <- sort(unique(weekdayData$interval))
splitweekdayDataByInterval <- split(weekdayData$steps, weekdayData$interval)
```


..and calculate the mean of steps for each interval..

```r
meanWeekendStepByInterval <- as.data.frame(sapply(splitweekendDataByInterval, 
    mean))
names(meanWeekendStepByInterval) <- c("Steps")

meanWeekdayStepByInterval <- as.data.frame(sapply(splitweekdayDataByInterval, 
    mean))
names(meanWeekdayStepByInterval) <- c("Steps")
```


Drawing the solution: 

Weekend

```r
plot(allIntervals, meanWeekendStepByInterval$Steps, type = "l", col = "blue", 
    main = "Weekend. steps mean", xlab = "5-interval", ylab = "Mean of steps")
```

![plot of chunk unnamed-chunk-37](figure/unnamed-chunk-37.png) 



```r
png(filename = "figures/plot6.png", width = 480, height = 480, units = "px")
plot(allIntervals, meanWeekendStepByInterval$Steps, type = "l", col = "blue", 
    main = "Weekend. steps mean", xlab = "5-interval", ylab = "Mean of steps")
dev.off()
```

```
## pdf 
##   2
```


Weekday

```r
plot(allIntervals, meanWeekdayStepByInterval$Steps, type = "l", col = "blue", 
    main = "Weekday. steps mean", xlab = "5-interval", ylab = "Mean of steps")
```

![plot of chunk unnamed-chunk-39](figure/unnamed-chunk-39.png) 



```r
png(filename = "figures/plot7.png", width = 480, height = 480, units = "px")
plot(allIntervals, meanWeekdayStepByInterval$Steps, type = "l", col = "blue", 
    main = "Weekend. steps mean", xlab = "5-interval", ylab = "Mean of steps")
dev.off()
```

```
## pdf 
##   2
```



## Second Method: lattice system graph



I'm going to create a DataFrame with the mean step group by Interval and factor Weekday or Weekend:


```r

weInterval <- unique(sort(weekendData$interval))
splitweekendDataIntervals <- split(weekendData$steps, weInterval)
meanSplitweekendDataIntervals <- as.data.frame(sapply(splitweekendDataIntervals, 
    mean))
names(meanSplitweekendDataIntervals) <- "meanSteps"
wEMatrixValues <- as.data.frame(cbind(meanSplitweekendDataIntervals$meanSteps, 
    weInterval, "Weekend"))
names(wEMatrixValues) <- c("meanSteps", "interval", "weekdate")

wdInterval <- unique(sort(weekdayData$interval))
splitweekdayDataIntervals <- split(weekdayData$steps, wdInterval)
meanSplitweekdayDataIntervals <- as.data.frame(sapply(splitweekdayDataIntervals, 
    mean))
names(meanSplitweekdayDataIntervals) <- "meanSteps"
wDMatrixValues <- as.data.frame(cbind(meanSplitweekdayDataIntervals$meanSteps, 
    wdInterval, "Weekday"))
names(wDMatrixValues) <- c("meanSteps", "interval", "weekdate")

meanStepMatrix <- as.data.frame(rbind(wDMatrixValues, wEMatrixValues))
meanStepMatrix$meanSteps <- as.numeric(as.character(meanStepMatrix$meanSteps))
meanStepMatrix$interval <- as.numeric(as.character(meanStepMatrix$interval))
```


This is the final dataframe:

```r
head(meanStepMatrix)
```

```
##   meanSteps interval weekdate
## 1    2.0806        0  Weekday
## 2    0.4583        5  Weekday
## 3    0.2139       10  Weekday
## 4    0.2361       15  Weekday
## 5    0.1472       20  Weekday
## 6    1.3694       25  Weekday
```


...and now i'm going to print it with 'lattice graph system':
 - First create and print it

```r
library("lattice")
p <- xyplot(meanStepMatrix$meanSteps ~ meanStepMatrix$interval | meanStepMatrix$weekdate, 
    meanStepMatrix, layout = c(1, 2), type = "l", xlab = "Interval", ylab = "Number of steps")
print(p)
```

![plot of chunk unnamed-chunk-43](figure/unnamed-chunk-43.png) 

 - and now safe it:

```r
png(filename = "figures/plot8.png", width = 480, height = 480, units = "px")
print(p)
dev.off()
```

```
## pdf 
##   2
```




















