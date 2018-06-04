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

unite()
-------

Here, we put two columns together by using the `unite()` function. Our data originally looks like this.

``` r
head(newData$venue_city)
```

    ## [1] MANSFIELD      QUINCY         PHOENIX        DALLAS        
    ## [5] AUBURN         SAN BERNARDINO
    ## 199 Levels: ABBOTSFORD AKRON ALBANY ALBUQUERQUE ALLEN ALPHARETTA ... YOUNGSTOWN

``` r
head(newData$venue_state)
```

    ## [1] MASSACHUSETTS WASHINGTON    ARIZONA       TEXAS         WASHINGTON   
    ## [6] CALIFORNIA   
    ## 48 Levels: ALABAMA ALBERTA ARIZONA ARKANSAS ... WISCONSIN

Now we combine the two columns

``` r
testData <- unite(newData, "venue_loc", venue_city, venue_state, sep = ", ")
head(testData$venue_loc)
```

    ## [1] "MANSFIELD, MASSACHUSETTS"   "QUINCY, WASHINGTON"        
    ## [3] "PHOENIX, ARIZONA"           "DALLAS, TEXAS"             
    ## [5] "AUBURN, WASHINGTON"         "SAN BERNARDINO, CALIFORNIA"

gather()
--------

``` r
testData <- spread(testData, timezn_nm, venue_loc)
testData <- select(testData, seq(ncol(testData) - 3, ncol(testData)))
head(testData)
```

    ##             CST                      EST              MST
    ## 1          <NA> MANSFIELD, MASSACHUSETTS             <NA>
    ## 2          <NA>                     <NA>             <NA>
    ## 3          <NA>                     <NA> PHOENIX, ARIZONA
    ## 4 DALLAS, TEXAS                     <NA>             <NA>
    ## 5          <NA>                     <NA>             <NA>
    ## 6          <NA>                     <NA>             <NA>
    ##                          PST
    ## 1                       <NA>
    ## 2         QUINCY, WASHINGTON
    ## 3                       <NA>
    ## 4                       <NA>
    ## 5         AUBURN, WASHINGTON
    ## 6 SAN BERNARDINO, CALIFORNIA

spread()
--------
