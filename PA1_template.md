# PA1_template
Madan  
Sunday, July 19, 2015  


# Loading and preprocessing the data
Show any code that is needed to
      
      1.Load the data (i.e. read.csv())

```r
raw_data<-read.csv("./activity.csv")
```
      2.Process/transform the data (if necessary) into a format suitable for your analysis

```r
totalsteps_data<-na.omit(raw_data)
```

# What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.
      
      1.Calculate the total number of steps taken per day

```r
totalsteps<-ddply(totalsteps_data,.(date),summarise,total_step=sum(steps))
```
      2.If you do not understand the difference between a histogram and a barplot, research the difference between them. Make a histogram of the total number of steps taken each day

```r
hist(totalsteps$total_step,xlab = "Total Steps",main="Histogram of the total number of steps taken each day")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 
     
     
     3.Calculate and report the mean and median of the total number of steps taken per day

```r
mean(totalsteps$total_step)
```

```
## [1] 10766.19
```

```r
median(totalsteps$total_step)
```

```
## [1] 10765
```

# What is the average daily activity pattern?
      
      1.Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
time_plot_data<-ddply(totalsteps_data,.(interval),summarise,avg_steps=mean(steps))
ggplot(time_plot_data,aes(interval,avg_steps))+geom_line()+xlab("5-minute interval")+ylab("Avg steps avged across all days")+ggtitle("Histogram of the total number of steps taken each day")
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png) 
     
     
     2.Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
time_plot_data$interval[which.max(time_plot_data$avg_steps)]
```

```
## [1] 835
```

# Imputing missing values
Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.
     
     
     1.Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
length(which(is.na(raw_data)))
```

```
## [1] 2304
```
      2.Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

```r
tdr_data<-tbl_df(raw_data)
tdr_data<- mutate(tdr_data,steps=replace(steps,is.na(steps),time_plot_data$avg_steps))
```
      3.Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
totalsteps_withoutNA<-ddply(tdr_data,.(date),summarise,total_step=sum(steps))
```
      4.Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
hist(totalsteps_withoutNA$total_step,main="Histogram of the total number of steps taken each day",xlab = "Total Steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-12-1.png) 

```r
mean(totalsteps_withoutNA$total_step)
```

```
## [1] 10766.19
```

```r
median(totalsteps_withoutNA$total_step)                                      
```

```
## [1] 10766.19
```
# Are there differences in activity patterns between weekdays and weekends?
For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

      
      1.Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.


      2.Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

```r
ggplot(diff_days,aes(x=interval,y=avg_steps))+geom_line()+facet_wrap(~wDay,nrow=2)+xlab("5-minute interval")+ylab("Avg steps avged across all days")+ggtitle("Time series Plot")
```

![](PA1_template_files/figure-html/unnamed-chunk-14-1.png) 

