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

    ##                Country Population Phones..per.1000.
    ## 109          Kiribati      105432              42.7
    ## 209 Turks & Caicos Is       21152             269.5
    ## 15       Bahamas. The      303770             460.6
    ## 90              Haiti     8308504              16.9
    ## 79          Gibraltar       27928             877.7
    ## 195         Swaziland     1136334              30.8
    ##                                  Region
    ## 109 OCEANIA                            
    ## 209             LATIN AMER. & CARIB    
    ## 15              LATIN AMER. & CARIB    
    ## 90              LATIN AMER. & CARIB    
    ## 79  WESTERN EUROPE                     
    ## 195 SUB-SAHARAN AFRICA

``` r
head(worldCountriesMix)
```

    ##     Phones..per.1000.       Country                              Region
    ## 117              23.7      Lesotho  SUB-SAHARAN AFRICA                 
    ## 90               16.9        Haiti              LATIN AMER. & CARIB    
    ## 195              30.8    Swaziland  SUB-SAHARAN AFRICA                 
    ## 121             223.4    Lithuania  BALTICS                            
    ## 15              460.6 Bahamas. The              LATIN AMER. & CARIB    
    ## 85               92.1    Guatemala              LATIN AMER. & CARIB    
    ##     Population
    ## 117    2022331
    ## 90     8308504
    ## 195    1136334
    ## 121    3585906
    ## 15      303770
    ## 85    12293545

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

    ##              Country Population Phones..per.1000.
    ## 1 Turks & Caicos Is       21152             269.5
    ## 2         Gibraltar       27928             877.7
    ## 3     Faroe Islands       47246             503.8
    ## 4          Kiribati      105432              42.7
    ## 5      Bahamas. The      303770             460.6
    ## 6         Swaziland     1136334              30.8
    ##                                Region
    ## 1             LATIN AMER. & CARIB    
    ## 2 WESTERN EUROPE                     
    ## 3 WESTERN EUROPE                     
    ## 4 OCEANIA                            
    ## 5             LATIN AMER. & CARIB    
    ## 6 SUB-SAHARAN AFRICA

``` r
head(worldCountries1 %>%
  arrange(Phones..per.1000.))
```

    ##             Country Population Phones..per.1000.
    ## 1            Niger    12525094               1.9
    ## 2 Papua New Guinea     5670544              10.9
    ## 3    Cote d'Ivoire    17654843              14.6
    ## 4            Haiti     8308504              16.9
    ## 5          Lesotho     2022331              23.7
    ## 6        Swaziland     1136334              30.8
    ##                                Region
    ## 1 SUB-SAHARAN AFRICA                 
    ## 2 OCEANIA                            
    ## 3 SUB-SAHARAN AFRICA                 
    ## 4             LATIN AMER. & CARIB    
    ## 5 SUB-SAHARAN AFRICA                 
    ## 6 SUB-SAHARAN AFRICA

We just saw the first use of the pipe operator! In English, we took the data frame `worldCountries1`, **then** arranged its data by the values in the `Phones..per.1000.` column. We can accomplish this task without using the pipe operator as well.

``` r
head(arrange(worldCountries1, Phones..per.1000.))
```

    ##             Country Population Phones..per.1000.
    ## 1            Niger    12525094               1.9
    ## 2 Papua New Guinea     5670544              10.9
    ## 3    Cote d'Ivoire    17654843              14.6
    ## 4            Haiti     8308504              16.9
    ## 5          Lesotho     2022331              23.7
    ## 6        Swaziland     1136334              30.8
    ##                                Region
    ## 1 SUB-SAHARAN AFRICA                 
    ## 2 OCEANIA                            
    ## 3 SUB-SAHARAN AFRICA                 
    ## 4             LATIN AMER. & CARIB    
    ## 5 SUB-SAHARAN AFRICA                 
    ## 6 SUB-SAHARAN AFRICA

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

    ## [1]  5670544  7385367  2022331    47246    21152 12293545

``` r
newData%>%
  between(200000, 7500000)
```

    ##  [1]  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE
    ## [12]  TRUE FALSE  TRUE FALSE

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

`desc()` is a function that is pretty straightforward. It takes every value in the vector and flips the sign. When this function is used in conjunction with `arrange()`, the vector returned is sorted in reverse order.

#### Usage

`desc(vec)`

#### Parameters

-   `vec`: a vector to reverse the signage of each element

#### Examples

``` r
head(worldCountries1$Phones..per.1000.)
```

    ## [1]  42.7 269.5 460.6  16.9 877.7  30.8

``` r
desc(worldCountries1$Phones..per.1000.) %>%
  head
```

    ## [1]  -42.7 -269.5 -460.6  -16.9 -877.7  -30.8

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

### I'm having issues with my `dplyr` installation! What do I do?

`dr_dplyr()` is a maintenance tool provided by `dplyr` to diagnose common problems with the package itself. This function can sometimes show false errors, so only use it if there are problems.

#### Usage

`dr_dplyr()`

#### Parameters

**N/A**

#### Examples

``` r
dr_dplyr()
```

### I need a more readable description of my tibble.

`explain()` is a function that is similar to `print()` and `str()` but is more focused on human readability while still providing more details about the data.

#### Usage

`explain(.data)`

#### Parameters

-   `.data`: data that needs explaining

#### Examples

``` r
#need to fix this
#explain(worldCountriesTbl)
```

### Is there an easy way to find elements that fit a certain condition?

Well, of course there is! It's called the `filter()` function. It is different from using subsets (like `[]`) because it does not return `NA` values.

#### Usage

`filter(.data, ...)`

#### Parameters

-   `.data`: data to pass through the filter
-   `...`:

#### Examples

``` r
head((worldCountries %>%
  filter(between(GDP....per.capita., 2000, 5000)))[1])
```

    ##       Country
    ## 1    Albania 
    ## 2    Armenia 
    ## 3 Azerbaijan 
    ## 4     Belize 
    ## 5    Bolivia 
    ## 6      China

### Grouping multiple rows together for batch operations

The `group_by()` function is used to easily perform operations such as `mean()` for all values that fall into a category. It is generally used with the `summarise()` function to show outputs by the generated groups.

#### Usage

`group_by(.data, ..., add = FALSE)`

#### Parameters

-   `.data`: object to group variables together
-   `...`: variables that will be grouped together
-   `add`: should this operation override existing groups?

#### Examples

``` r
worldCountriesTbl %>%
  group_by(Region) %>%
  summarise(mean(Population))
```

    ## # A tibble: 11 x 2
    ##    Region                                `mean(Population)`
    ##    <fct>                                              <dbl>
    ##  1 "ASIA (EX. NEAR EAST)         "               131713651.
    ##  2 "BALTICS                            "           2394991.
    ##  3 "C.W. OF IND. STATES "                         23340129 
    ##  4 "EASTERN EUROPE                     "           9992893.
    ##  5 "LATIN AMER. & CARIB    "                      12484991.
    ##  6 "NEAR EAST                          "          12191774.
    ##  7 "NORTHERN AFRICA                    "          26901189.
    ##  8 "NORTHERN AMERICA                   "          66334461.
    ##  9 "OCEANIA                            "           1577698.
    ## 10 "SUB-SAHARAN AFRICA                 "          14694843.
    ## 11 "WESTERN EUROPE                     "          14155000.

### Joining multiple tbls into one

Sometimes you have two tbls with related data. You could continually perform operations by referring to each separately, but one can use the `join()` function to place them into one.

#### Usage

`inner_join(left, right, by = NULL, copy = FALSE, suffix = c(".x", ".y"), ...)` `left_join(left, right, by = NULL, copy = FALSE, suffix = c(".x", ".y"), ...)` `right_join(left, right, by = NULL, copy = FALSE, suffix = c(".x", ".y"), ...)` `full_join(left, right, by = NULL, copy = FALSE, suffix = c(".x", ".y"), ...)` `semi_join(left, right, by = NULL, copy = FALSE, ...)` `anti_join(left, right, by = NULL, copy = FALSE, ...)`

#### Parameters

-   `left`: a tbl
-   `right`: a tbl
-   `by`: a character vector of column names to join by. if this remains `NULL`, then this function will join the tbls by columns that have the same name.
-   `copy`: if `TRUE`, then `right` will be copied into the same data source as `left`
-   `suffix`: specify in the output whether there are any other duplicate columns that weren't joined

#### Examples

``` r
worldCountriesNew <- worldCountries[ , c("Area..sq..mi..", "Net.migration", "Arable....")]
worldCountries1 <- worldCountries[ , c("Other....", "Region", "Crops....", "Arable....", "Phones..per.1000.", "Population")]
head(full_join(worldCountries1, worldCountriesNew))
```

    ## Joining, by = "Arable...."

    ##   Other....                              Region Crops.... Arable....
    ## 1     87.65       ASIA (EX. NEAR EAST)               0.22      12.13
    ## 2     74.49 EASTERN EUROPE                           4.42      21.09
    ## 3     96.53 NORTHERN AFRICA                          0.25       3.22
    ## 4     75.00 OCEANIA                                 15.00      10.00
    ## 5     75.00 OCEANIA                                 15.00      10.00
    ## 6     97.78 WESTERN EUROPE                           0.00       2.22
    ##   Phones..per.1000. Population Area..sq..mi.. Net.migration
    ## 1               3.2   31056997         647500         23.06
    ## 2              71.2    3581655          28748         -4.93
    ## 3              78.1   32930091        2381740         -0.39
    ## 4             259.5      57794            199        -20.71
    ## 5             259.5      57794            960         -0.41
    ## 6             497.2      71201            468          6.60

### I need to add some extra columns based on existing data for future use

The `mutate()` function is great for creating extra columns in a data frame

#### Usage

#### Parameters

#### Examples

``` r
head(worldCountries1 %>%
  mutate("Total Phones" = Phones..per.1000. * Population * 1000))
```

    ##   Other....                              Region Crops.... Arable....
    ## 1     87.65       ASIA (EX. NEAR EAST)               0.22      12.13
    ## 2     74.49 EASTERN EUROPE                           4.42      21.09
    ## 3     96.53 NORTHERN AFRICA                          0.25       3.22
    ## 4     75.00 OCEANIA                                 15.00      10.00
    ## 5     97.78 WESTERN EUROPE                           0.00       2.22
    ## 6     97.35 SUB-SAHARAN AFRICA                       0.24       2.41
    ##   Phones..per.1000. Population Total Phones
    ## 1               3.2   31056997 9.938239e+10
    ## 2              71.2    3581655 2.550138e+11
    ## 3              78.1   32930091 2.571840e+12
    ## 4             259.5      57794 1.499754e+10
    ## 5             497.2      71201 3.540114e+10
    ## 6               7.8   12127071 9.459115e+10

### 

n()

#### Usage

#### Parameters

#### Examples

``` r
worldCountries %>%
  summarise(n())
```

    ##   n()
    ## 1 227

``` r
worldCountries1 %>%
  summarise(n())
```

    ##   n()
    ## 1 227

### 

na\_if()

#### Usage

#### Parameters

#### Examples

``` r
x <- c(1:7, 3, 3)
x <- sample(x)
na_if(x, 3)
```

    ## [1]  6  5  1  4 NA  7  2 NA NA

### 

#### Usage

#### Parameters

#### Examples

``` r
y <- (x + .073)
near(x, y)
```

    ## [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE

``` r
y <- (x + .000000003)
near(x, y)
```

    ## [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE

### 

#### Usage

#### Parameters

#### Examples

``` r
n_distinct(x)
```

    ## [1] 7

### 

#### Usage

#### Parameters

#### Examples

``` r
summarise(starwars, n())
```

    ## # A tibble: 1 x 1
    ##   `n()`
    ##   <int>
    ## 1    87

### 

#### Usage

#### Parameters

#### Examples

``` r
head(worldCountries %>%
       pull(Population))
```

    ## [1] 31056997  3581655 32930091    57794    71201 12127071

### 

#### Usage

#### Parameters

#### Examples

``` r
head(x)
```

    ## [1] 6 5 1 4 3 7

``` r
y <- sample(x)
head(y)
```

    ## [1] 5 2 4 6 1 7

### 

#### Usage

#### Parameters

#### Examples

``` r
head(select(sample_n(starwars, 6), c("name", "species")))
```

    ## # A tibble: 6 x 2
    ##   name           species   
    ##   <chr>          <chr>     
    ## 1 Adi Gallia     Tholothian
    ## 2 Nute Gunray    Neimodian 
    ## 3 Shmi Skywalker Human     
    ## 4 Owen Lars      Human     
    ## 5 Jocasta Nu     Human     
    ## 6 Arvel Crynyd   Human

### 

#### Usage

#### Parameters

#### Examples

``` r
head(starwars)
```

    ## # A tibble: 6 x 13
    ##   name      height  mass hair_color skin_color eye_color birth_year gender
    ##   <chr>      <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> 
    ## 1 Luke Sky~    172    77 blond      fair       blue            19   male  
    ## 2 C-3PO        167    75 <NA>       gold       yellow         112   <NA>  
    ## 3 R2-D2         96    32 <NA>       white, bl~ red             33   <NA>  
    ## 4 Darth Va~    202   136 none       white      yellow          41.9 male  
    ## 5 Leia Org~    150    49 brown      light      brown           19   female
    ## 6 Owen Lars    178   120 brown, gr~ light      blue            52   male  
    ## # ... with 5 more variables: homeworld <chr>, species <chr>, films <list>,
    ## #   vehicles <list>, starships <list>

``` r
slice(starwars, c(1, 3, 4))
```

    ## # A tibble: 3 x 13
    ##   name      height  mass hair_color skin_color eye_color birth_year gender
    ##   <chr>      <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> 
    ## 1 Luke Sky~    172    77 blond      fair       blue            19   male  
    ## 2 R2-D2         96    32 <NA>       white, bl~ red             33   <NA>  
    ## 3 Darth Va~    202   136 none       white      yellow          41.9 male  
    ## # ... with 5 more variables: homeworld <chr>, species <chr>, films <list>,
    ## #   vehicles <list>, starships <list>

### 

#### Usage

#### Parameters

#### Examples

### 

#### Usage

#### Parameters

#### Examples

### 

#### Usage

#### Parameters

#### Examples

### 

#### Usage

#### Parameters

#### Examples

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

all\_vars arrange\_all as.table.tbl\_cube as.tbl\_cube auto\_copy bind case\_when compute copy\_to cumall do filter\_all funs groups group\_by\_all ident if\_else join.tbl\_df lead-lag mutate na\_if near nth order\_by ranking recode rowwise scoped select\_all select\_vars setops sql src\_dbi summarise summarise\_all tally tbl tbl\_cube top\_n ungroup vars
