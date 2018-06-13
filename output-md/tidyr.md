Using tidyr: A Short Demo
================
Shiv Sundar
May 30, 2018

Introduction
============

In this demo, we will take a look at a package called `tidyr` and its most used functions. `tidyr` is a tool used for cleaning up data for ease of use later.

Getting our relevant data
-------------------------

Here's what our data looks like currently:

``` r
head(data)
```

    ##                         name           order_genus  vore conservation
    ## 1                    Cheetah   Carnivora, Acinonyx carni           lc
    ## 2                 Owl monkey       Primates, Aotus  omni         <NA>
    ## 3            Mountain beaver  Rodentia, Aplodontia herbi           nt
    ## 4 Greater short-tailed shrew Soricomorpha, Blarina  omni           lc
    ## 5                        Cow     Artiodactyla, Bos herbi domesticated
    ## 6           Three-toed sloth      Pilosa, Bradypus herbi         <NA>
    ##   sleep_total sleep_cycle awake brainwt  bodywt rem_ratio
    ## 1        12.1          NA  11.9      NA  50.000        NA
    ## 2        17.0          NA   7.0 0.01550   0.480 0.1058824
    ## 3        14.4          NA   9.6      NA   1.350 0.1666667
    ## 4        14.9   0.1333333   9.1 0.00029   0.019 0.1543624
    ## 5         4.0   0.6666667  20.0 0.42300 600.000 0.1750000
    ## 6        14.4   0.7666667   9.6      NA   3.850 0.1527778

This data, while mostly organized, is not "clean". We can use the `tidyr` package to accomplish this goal.

separate()
----------

In this data, the `latin_name` column has data that we can split using the `separate()` function. This way, it will be easier to find out how many *genera* fall into each diet type.

``` r
data <- data %>%
  separate(order_genus, c("genus", "order"), sep = ", ", remove = TRUE)
head(data)
```

    ##                         name        genus      order  vore conservation
    ## 1                    Cheetah    Carnivora   Acinonyx carni           lc
    ## 2                 Owl monkey     Primates      Aotus  omni         <NA>
    ## 3            Mountain beaver     Rodentia Aplodontia herbi           nt
    ## 4 Greater short-tailed shrew Soricomorpha    Blarina  omni           lc
    ## 5                        Cow Artiodactyla        Bos herbi domesticated
    ## 6           Three-toed sloth       Pilosa   Bradypus herbi         <NA>
    ##   sleep_total sleep_cycle awake brainwt  bodywt rem_ratio
    ## 1        12.1          NA  11.9      NA  50.000        NA
    ## 2        17.0          NA   7.0 0.01550   0.480 0.1058824
    ## 3        14.4          NA   9.6      NA   1.350 0.1666667
    ## 4        14.9   0.1333333   9.1 0.00029   0.019 0.1543624
    ## 5         4.0   0.6666667  20.0 0.42300 600.000 0.1750000
    ## 6        14.4   0.7666667   9.6      NA   3.850 0.1527778

unite()
-------

Here, we put two columns together by using the `unite()` function. Our data originally looks like this.

``` r
head(data$vore)
```

    ## [1] carni omni  herbi omni  herbi herbi
    ## Levels: carni herbi insecti omni

The formatting of this data isn't easy to understand. What we can do is create a column of the same

``` r
data$suffix = "vore"
data <- data %>%
  unite("diet", vore, suffix, sep = "", remove = TRUE)
head(data)
```

    ##                         name        genus      order      diet
    ## 1                    Cheetah    Carnivora   Acinonyx carnivore
    ## 2                 Owl monkey     Primates      Aotus  omnivore
    ## 3            Mountain beaver     Rodentia Aplodontia herbivore
    ## 4 Greater short-tailed shrew Soricomorpha    Blarina  omnivore
    ## 5                        Cow Artiodactyla        Bos herbivore
    ## 6           Three-toed sloth       Pilosa   Bradypus herbivore
    ##   conservation sleep_total sleep_cycle awake brainwt  bodywt rem_ratio
    ## 1           lc        12.1          NA  11.9      NA  50.000        NA
    ## 2         <NA>        17.0          NA   7.0 0.01550   0.480 0.1058824
    ## 3           nt        14.4          NA   9.6      NA   1.350 0.1666667
    ## 4           lc        14.9   0.1333333   9.1 0.00029   0.019 0.1543624
    ## 5 domesticated         4.0   0.6666667  20.0 0.42300 600.000 0.1750000
    ## 6         <NA>        14.4   0.7666667   9.6      NA   3.850 0.1527778

spread()
--------

Looking at our `worldCountries` data frame, we find that it is extremely long and contains repeated observations

``` r
filter(worldCountries, Country %in% levels(worldCountries$Country)[1:4])
```

    ##            Country  Land Type (%) Measurement
    ## 1     Afghanistan  Area..sq..mi..   647500.00
    ## 2         Albania  Area..sq..mi..    28748.00
    ## 3         Algeria  Area..sq..mi..  2381740.00
    ## 4  American Samoa  Area..sq..mi..      199.00
    ## 5     Afghanistan      Arable....       12.13
    ## 6         Albania      Arable....       21.09
    ## 7         Algeria      Arable....        3.22
    ## 8  American Samoa      Arable....       10.00
    ## 9     Afghanistan       Crops....        0.22
    ## 10        Albania       Crops....        4.42
    ## 11        Algeria       Crops....        0.25
    ## 12 American Samoa       Crops....       15.00
    ## 13    Afghanistan       Other....       87.65
    ## 14        Albania       Other....       74.49
    ## 15        Algeria       Other....       96.53
    ## 16 American Samoa       Other....       75.00

To fix this, we can use the `spread()` function.

``` r
worldCountriesWide <- worldCountries %>%
  spread(`Land Type (%)`, Measurement)
head(worldCountriesWide)
```

    ##           Country Arable.... Area..sq..mi.. Crops.... Other....
    ## 1    Afghanistan       12.13         647500      0.22     87.65
    ## 2        Albania       21.09          28748      4.42     74.49
    ## 3        Algeria        3.22        2381740      0.25     96.53
    ## 4 American Samoa       10.00            199     15.00     75.00
    ## 5        Andorra        2.22            468      0.00     97.78
    ## 6         Angola        2.41        1246700      0.24     97.35

In the previous example, we saw a way to convert a *long* data frame to a *wide* data frame. We can do the exact opposite with the `gather()` function. \#\#gather()

``` r
worldCountriesLong <- worldCountriesWide %>%
  gather("Land Type (%)", "Measurement", -Country)
head(worldCountriesLong)
```

    ##           Country Land Type (%) Measurement
    ## 1    Afghanistan     Arable....       12.13
    ## 2        Albania     Arable....       21.09
    ## 3        Algeria     Arable....        3.22
    ## 4 American Samoa     Arable....       10.00
    ## 5        Andorra     Arable....        2.22
    ## 6         Angola     Arable....        2.41
