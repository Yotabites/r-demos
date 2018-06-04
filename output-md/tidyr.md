Using tidyr: A Short Demo
================
Shiv Sundar
May 30, 2018

Introduction
============

In this demo, we will take a look at a package called `tidyr` and its most used functions. To do this, let's assume the role of

separate()
----------

The data in the column titled `event_date_time` contains repeat data from the `event_date` column. We can remove this duplicate data by using the `separate()` command to split it into two columns.

``` r
newData <- separate(data, event_date_time, c("event_dt", "event_time"), sep = " ")
head(newData$event_dt)
```

    ## [1] "2015-09-12" "2009-09-05" "2006-04-22" "2011-09-03" "2005-07-31"
    ## [6] "2012-07-22"

``` r
head(newData$event_time)
```

    ## [1] "23:30:00" "01:00:00" "01:30:00" "00:00:00" "01:00:00" "02:00:00"

gather()
--------

unite()
-------

spread()
--------
