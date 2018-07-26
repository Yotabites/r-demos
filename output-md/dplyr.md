Using dplyr: A Short Demo
================
Shiv Sundar
May 22, 2018

Introduction
============

In this demo, let's pretend that our research question is whether ownership of phones and literacy rates are related when looking at regions. For our research, we will apply some transformations to our data and clean it up to get valid data. We'd probably be laughed at by the research community if we used unnecessary or skewed data in our research.

Important Information
---------------------

One of the most used tools provided by `dplyr` is the pipe (`%>%`) operator. We can use it to chain together multiple functions. It's kind of like saying: do this, **then** do this, **then** do this. We'll cover the use and syntax for it as we go.

Basic functions in `dplyr`
==========================

Let's have a quick look at our data. All data can be downloaded from [this](https://github.com/shiv-sundar/YotabitesRPackageDemos/tree/master/data) repository.

``` r
head(data)
```

    ##           Country                              Region Population
    ## 1    Afghanistan        ASIA (EX. NEAR EAST)            31056997
    ## 2        Albania  EASTERN EUROPE                         3581655
    ## 3        Algeria  NORTHERN AFRICA                       32930091
    ## 4 American Samoa  OCEANIA                                  57794
    ## 5        Andorra  WESTERN EUROPE                           71201
    ## 6         Angola  SUB-SAHARAN AFRICA                    12127071
    ##   Area..sq..mi.. Pop..Density..per.sq..mi.. Coastline..coast.area.ratio.
    ## 1         647500                       48.0                         0.00
    ## 2          28748                      124.6                         1.26
    ## 3        2381740                       13.8                         0.04
    ## 4            199                      290.4                        58.29
    ## 5            468                      152.1                         0.00
    ## 6        1246700                        9.7                         0.13
    ##   Net.migration Infant.mortality..per.1000.births. GDP....per.capita.
    ## 1         23.06                             163.07                700
    ## 2         -4.93                              21.52               4500
    ## 3         -0.39                              31.00               6000
    ## 4        -20.71                               9.27               8000
    ## 5          6.60                               4.05              19000
    ## 6          0.00                             191.19               1900
    ##   Literacy.... Phones..per.1000. Arable.... Crops.... Other.... Climate
    ## 1         36.0               3.2      12.13      0.22     87.65       1
    ## 2         86.5              71.2      21.09      4.42     74.49       3
    ## 3         70.0              78.1       3.22      0.25     96.53       1
    ## 4         97.0             259.5      10.00     15.00     75.00       2
    ## 5        100.0             497.2       2.22      0.00     97.78       3
    ## 6         42.0               7.8       2.41      0.24     97.35      NA
    ##   Birthrate Deathrate Agriculture Industry Service
    ## 1     46.60     20.34       0.380    0.240   0.380
    ## 2     15.11      5.22       0.232    0.188   0.579
    ## 3     17.14      4.61       0.101    0.600   0.298
    ## 4     22.46      3.27          NA       NA      NA
    ## 5      8.71      6.25          NA       NA      NA
    ## 6     45.11     24.20       0.096    0.658   0.246

Here we can see that our data consists of quite a bit of information, sorted by the name of each country.

Getting data for our study
--------------------------

First let's begin by selecting the relevant data that we need to start out.

``` r
newData <- select(data, Country, Phones..per.1000., Region, Literacy....)
```

We just placed the relevant data for our question in a new data frame, `newData`. Let's check out our new data frame.

``` r
head(newData)
```

    ##           Country Phones..per.1000.                              Region
    ## 1    Afghanistan                3.2       ASIA (EX. NEAR EAST)         
    ## 2        Albania               71.2 EASTERN EUROPE                     
    ## 3        Algeria               78.1 NORTHERN AFRICA                    
    ## 4 American Samoa              259.5 OCEANIA                            
    ## 5        Andorra              497.2 WESTERN EUROPE                     
    ## 6         Angola                7.8 SUB-SAHARAN AFRICA                 
    ##   Literacy....
    ## 1         36.0
    ## 2         86.5
    ## 3         70.0
    ## 4         97.0
    ## 5        100.0
    ## 6         42.0

Now let's take a quick look at how we can accomplish this same call using the pipe operator.

``` r
newDataPipe <- data %>%
  select(Country, Phones..per.1000., Region, Literacy....)
```

Here we see how the pipe operator works. In English, we took the data frame `data`, **then** selected the relevant columns, and placed it into the data frame `newDataPipe`. Let's look at this data frame. Notice that it is the exact same as `newData`.

``` r
head(newDataPipe)
```

    ##           Country Phones..per.1000.                              Region
    ## 1    Afghanistan                3.2       ASIA (EX. NEAR EAST)         
    ## 2        Albania               71.2 EASTERN EUROPE                     
    ## 3        Algeria               78.1 NORTHERN AFRICA                    
    ## 4 American Samoa              259.5 OCEANIA                            
    ## 5        Andorra              497.2 WESTERN EUROPE                     
    ## 6         Angola                7.8 SUB-SAHARAN AFRICA                 
    ##   Literacy....
    ## 1         36.0
    ## 2         86.5
    ## 3         70.0
    ## 4         97.0
    ## 5        100.0
    ## 6         42.0

Verbs provided by `dplyr` support both options that we just covered. It's up to your preference. However, in this demo we will use the pipe operator to familiarize you with its use.

Sorting data to have more valuable information first
----------------------------------------------------

Next, we need to know which countries have the most phones. We can accomplish this by using the `arrange()` function to sort our data.

``` r
newData %>%
  arrange(desc(Phones..per.1000.)) %>%
  select(Country, Phones..per.1000.) %>%
  head
```

    ##           Country Phones..per.1000.
    ## 1         Monaco             1035.6
    ## 2  United States              898.0
    ## 3      Gibraltar              877.7
    ## 4        Bermuda              851.4
    ## 5       Guernsey              842.4
    ## 6 Cayman Islands              836.3

Apparently Monaco has more phones than people! That's quite interesting!

Separating our data by region
-----------------------------

Now, let's find all of the regions that are considered by this data.

``` r
distinct(newData, Region)
```

    ##                                 Region
    ## 1        ASIA (EX. NEAR EAST)         
    ## 2  EASTERN EUROPE                     
    ## 3  NORTHERN AFRICA                    
    ## 4  OCEANIA                            
    ## 5  WESTERN EUROPE                     
    ## 6  SUB-SAHARAN AFRICA                 
    ## 7              LATIN AMER. & CARIB    
    ## 8                 C.W. OF IND. STATES 
    ## 9  NEAR EAST                          
    ## 10 NORTHERN AMERICA                   
    ## 11 BALTICS

We see that we have 11 different regions.

Taking out invalid data
-----------------------

Some of our data contains empty values. We don't want those to skew our results! Let's get rid of those unneeded data points.

``` r
newData <- newData %>%
  filter(!is.na(Phones..per.1000.)) %>%
  filter(!is.na(Country)) %>%
  filter(!is.na(Literacy....)) %>%
  filter(!is.na(Region))
```

Now let's look at what happened in this code.

``` r
nrow(newDataPipe)
```

    ## [1] 227

``` r
nrow(newData)
```

    ## [1] 206

Here, we can see that we removed a total of 21 rows. That's quite a bit of data that could have skewed our results!

Introducing the summarise() function
------------------------------------

We want to look at the number of phones that regions have. However our data points are given to us by the country, not the region. Let's change that by using the `summarise()` function. This gives us a nice data frame of the function that we apply inside of it. Let's find out the average number of phones countries in the Eastern European region have.

``` r
newData[grep("EASTERN EUROPE", newData$Region), ] %>%
  summarise(sum(Phones..per.1000.))
```

    ##   sum(Phones..per.1000.)
    ## 1                 2673.5

This gives us the mean number of phones per 1000 people in the Eastern European region.

Grouping our data by region
---------------------------

Now let's apply that same transformation to all of our data. `group_by()` is a great function for this! It follows a group-apply-combine process. First, `group_by()` splits our original data frame into multiple data frame based on our parameter. Then, it allows us to apply a transformation to each of those frames. Lastly, it puts all of these data frames back into one. Let's apply our `summarise()` transformation to our data frame called `avgDataByReg`.

``` r
avgDataByReg <- newData %>%
  group_by(Region) %>%
  summarise(avgPhones=sum(Phones..per.1000.))
head(avgDataByReg)
```

    ## # A tibble: 6 x 2
    ##   Region                                avgPhones
    ##   <fct>                                     <dbl>
    ## 1 "ASIA (EX. NEAR EAST)         "           4621.
    ## 2 "BALTICS                            "      879.
    ## 3 "C.W. OF IND. STATES "                    1966.
    ## 4 "EASTERN EUROPE                     "     2674.
    ## 5 "LATIN AMER. & CARIB    "                11300.
    ## 6 "NEAR EAST                          "     2747.

Now, let's do the same to our literacy rates.

``` r
avgDataByReg$avgLit = (newData %>%
  group_by(Region) %>%
  summarise(avgLit = mean(Literacy....)))$avgLit
head(avgDataByReg)
```

    ## # A tibble: 6 x 3
    ##   Region                                avgPhones avgLit
    ##   <fct>                                     <dbl>  <dbl>
    ## 1 "ASIA (EX. NEAR EAST)         "           4621.   80.3
    ## 2 "BALTICS                            "      879.   99.7
    ## 3 "C.W. OF IND. STATES "                    1966.   98.7
    ## 4 "EASTERN EUROPE                     "     2674.   97.1
    ## 5 "LATIN AMER. & CARIB    "                11300.   90.5
    ## 6 "NEAR EAST                          "     2747.   78.1

Finalizing our data
-------------------

Now we get to the last function to cover on our list, `mutate()`. Using this, we can find the ratio of literacy percentage to phone ownership percentage. We do this by "mutating" the `avgLit` and `avgPhones` columns into a new column.

``` r
avgDataByReg %>%
  mutate(litToPhone = avgLit / (avgPhones/10))
```

    ## # A tibble: 11 x 4
    ##    Region                                avgPhones avgLit litToPhone
    ##    <fct>                                     <dbl>  <dbl>      <dbl>
    ##  1 "ASIA (EX. NEAR EAST)         "           4621.   80.3     0.174 
    ##  2 "BALTICS                            "      879.   99.7     1.14  
    ##  3 "C.W. OF IND. STATES "                    1966.   98.7     0.502 
    ##  4 "EASTERN EUROPE                     "     2674.   97.1     0.363 
    ##  5 "LATIN AMER. & CARIB    "                11300.   90.5     0.0801
    ##  6 "NEAR EAST                          "     2747.   78.1     0.284 
    ##  7 "NORTHERN AFRICA                    "      501    67.2     1.34  
    ##  8 "NORTHERN AMERICA                   "     2985.   97.8     0.327 
    ##  9 "OCEANIA                            "     3729.   88.8     0.238 
    ## 10 "SUB-SAHARAN AFRICA                 "     2118.   62.5     0.295 
    ## 11 "WESTERN EUROPE                     "    12915.   98.4     0.0762

This shows us that in certain regions, such as Northern Africa, a larger ratio of people can read than own phones. In the other end, there are many regions that have a smaller portion of people that can read compared to phones owned.
