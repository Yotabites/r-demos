`dplyr`: The Yotabites Demo
================
Shiv Sundar
June 5, 2018

Introduction
============

This tutorial was created to inform the user about every function provided by the `dplyr` package. Commonly used and simple functions will include a short use case and demo.

The pipe operator
-----------------

One of the most used tools provided by `dplyr` is the pipe (`%>%`) operator. We can use it to chain together multiple functions. It's kind of like saying: do this, **then** do this, **then** do this. We'll cover the use and syntax for it as we go.

Commonly used `dplyr` functions
-------------------------------

### Are these two data frames the same?

The `all_equal()` function is used to compare two data frames. One can use this function to check for equality without considering row order, column order, or both.

#### Usage

`all_equal(df1, df2, ignore_col_order = TRUE, ignore_row_order = TRUE, convert = FALSE, ...)`

#### Parameters

-   `df1`, `df2`: dataframes to compare.
-   `ignore_col_order`: should the ordering of columns be ignored?
-   `ignore_row_order`: should the ordering of rows be ignored?
-   `convert`: should the function attempt to coerce factors to characters and integers to doubles?
-   `...`: **not used**. required to comply with `all.equal()` usage.

#### Examples

Here we see what the data looks like.

``` r
head(worldCountries1)
```

    ##            Country Population Phones..per.1000.
    ## 120 Liechtenstein       33987             585.5
    ## 1     Afghanistan    31056997               3.2
    ## 70         France    60876136             586.4
    ## 177         Samoa      176908              75.2
    ## 212       Ukraine    46710816             259.9
    ## 192     Sri Lanka    20222240              61.5

``` r
head(worldCountriesMix)
```

    ##     Phones..per.1000. Population     Country
    ## 217              62.9   27307134 Uzbekistan 
    ## 77              667.9   82422299    Germany 
    ## 61              131.8   78887007      Egypt 
    ## 70              586.4   60876136     France 
    ## 130             505.0     400214      Malta 
    ## 198             153.8   18881361      Syria

Here we test different variations of the function.

``` r
all_equal(worldCountries1, worldCountriesMix)
```

    ## [1] TRUE

``` r
all_equal(worldCountries1, worldCountriesMix, TRUE, FALSE)
```

    ## [1] "Same row values, but different order"

``` r
all_equal(worldCountries1, worldCountriesMix, FALSE, FALSE)
```

    ## [1] "Same column names, but different order"

### I want to sort my data by a specified order!

Data can be sorted by using the `arrange()` function. It sorts using by using the natural order of the data type. Factors are normally sorted by alphabetical order. To order them differently, please use the `levels()` function.

#### Usage

`arrange(.data, ...)`

#### Parameters

-   `.data`: the dataframe to arrange
-   `...`: the columns to sort by

#### Examples

``` r
head(arrange(worldCountries1, Population))
```

    ##          Country Population Phones..per.1000.
    ## 1 Liechtenstein       33987             585.5
    ## 2         Samoa      176908              75.2
    ## 3 French Guiana      199509             255.6
    ## 4       Vanuatu      208869              32.6
    ## 5         Malta      400214             505.0
    ## 6         Haiti     8308504              16.9

``` r
head(worldCountries1 %>%
  arrange(Phones..per.1000.))
```

    ##        Country Population Phones..per.1000.
    ## 1 Afghanistan    31056997               3.2
    ## 2       Haiti     8308504              16.9
    ## 3     Vanuatu      208869              32.6
    ## 4   Sri Lanka    20222240              61.5
    ## 5  Uzbekistan    27307134              62.9
    ## 6       Samoa      176908              75.2

We just saw the first use of the pipe operator! In English, we took the data frame `worldCountries1`, **then** arranged its data by the values in the `Phones..per.1000.` column. We can accomplish this task without using the pipe operator as well.

``` r
head(arrange(worldCountries1, Phones..per.1000.))
```

    ##        Country Population Phones..per.1000.
    ## 1 Afghanistan    31056997               3.2
    ## 2       Haiti     8308504              16.9
    ## 3     Vanuatu      208869              32.6
    ## 4   Sri Lanka    20222240              61.5
    ## 5  Uzbekistan    27307134              62.9
    ## 6       Samoa      176908              75.2

Functions provided by `dplyr` support both options that we just covered. It's up to your preference. However, in this demo we will use the pipe operator to familiarize you with its use.

### Which values in a vector fall between 10.4 and 73?

How can we answer this question? Well, `dplyr` actually provides us with a function called `between()`. That's convenient!

#### Usage

`between(vector, min, max)`

#### Parameters

-   `vector`: the values to check
-   `min`: the minimum value (inclusive)
-   `max`: the maximum value (inclusive)

#### Examples

``` r
newData <- sample_n(worldCountriesMix, 15)$Population
head(newData)
```

    ## [1]   199509 32930091   176908    33987   400214 27307134

``` r
newData%>%
  between(200000, 7500000)
```

    ##  [1] FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE  TRUE FALSE
    ## [12] FALSE FALSE FALSE FALSE

### Getting rid of NAs and replacing them with values in a vector

The `coalesce()` function takes multiple vectors and combines places the first non-NA value in the returned vector.

#### Usage

`coalesce(firstVect, ...)`

#### Parameters

-   `firstVect`: the base vector. One can take this to be the vector that will be returned with its NA values filled if possible
-   `...`: any vector that is of length 1 or equal to the length of `firstVect`

#### Usage

``` r
x <- c(1, 6, NA, 3, NA, 4, 2, NA, 5)
y <- c(NA, 2, 5, NA, 3, NA, 6, 4, 1)
coalesce(x, y)
```

    ## [1] 1 6 5 3 3 4 2 4 5

``` r
coalesce(y, x)
```

    ## [1] 1 2 5 3 3 4 6 4 1

### `arrange()`, but backwards!

`desc()` is a function that is pretty straightforward. It takes every value in the vector and flips the sign. When this function is used in conjunction with `arrange()`, the vector returned by arrange is sorted in reverse order.

#### Usage

`desc(vec)`

#### Parameters

-   `vec`: a vector to reverse the signage of each element

#### Examples

``` r
head(worldCountries1$Phones..per.1000.)
```

    ## [1] 585.5   3.2 586.4  75.2 259.9  61.5

``` r
desc(worldCountries1$Phones..per.1000.) %>%
  head
```

    ## [1] -585.5   -3.2 -586.4  -75.2 -259.9  -61.5

### Ew! I have replicated data in my table!

A tool provided by `dplyr` that is frequently used for removing duplicate values is the `distinct()` function. It returns a vector with the duplicates removed

#### Usage

`distinct(.data, ..., .keep_all = FALSE)`

#### Parameters

-   `.data`: the tibble to remove values from (at least one is required)
-   `...`: other variables to check for uniqueness
-   `.keep_all`: should all variables in `.data` be kept?

#### Usage

``` r
worldCountriesTbl <- as_tibble(worldCountries)
worldCountriesTbl %>%
  distinct(Coastline..coast.area.ratio.) %>%
  head
```

    ## # A tibble: 6 x 1
    ##   Coastline..coast.area.ratio.
    ##                          <dbl>
    ## 1                         0   
    ## 2                         1.26
    ## 3                         0.04
    ## 4                        58.3 
    ## 5                         0.13
    ## 6                        59.8

Datasets provided by `dplyr`
----------------------------

These datasets are bundled with the `dplyr` package for testing and manipulating.

-   `band_members`
-   `band_instruments`
-   `band_instruments2`
-   `nasa`
-   `starwars`
-   `storms`

Other functions not covered
---------------------------

all\_vars arrange\_all as.table.tbl\_cube as.tbl\_cube auto\_copy bind case\_when compute copy\_to cumall distinct do dr\_dplyr explain filter filter\_all funs groups group\_by group\_by\_all ident if\_else join join.tbl\_df lead-lag mutate n na\_if near nth n\_distinct order\_by pull ranking recode rowwise sample scoped select select\_all select\_vars setops slice sql src\_dbi summarise summarise\_all tally tbl tbl\_cube top\_n vars
