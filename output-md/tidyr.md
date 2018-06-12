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

    ##                         name           latin_name  vore conservation
    ## 1                    Cheetah   Acinonyx Carnivora carni           lc
    ## 2                 Owl monkey       Aotus Primates  omni         <NA>
    ## 3            Mountain beaver  Aplodontia Rodentia herbi           nt
    ## 4 Greater short-tailed shrew Blarina Soricomorpha  omni           lc
    ## 5                        Cow     Bos Artiodactyla herbi domesticated
    ## 6           Three-toed sloth      Bradypus Pilosa herbi         <NA>
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
  separate(latin_name, c("genus", "order"), sep = " ", remove = TRUE)
head(data)
```

    ##                         name      genus        order  vore conservation
    ## 1                    Cheetah   Acinonyx    Carnivora carni           lc
    ## 2                 Owl monkey      Aotus     Primates  omni         <NA>
    ## 3            Mountain beaver Aplodontia     Rodentia herbi           nt
    ## 4 Greater short-tailed shrew    Blarina Soricomorpha  omni           lc
    ## 5                        Cow        Bos Artiodactyla herbi domesticated
    ## 6           Three-toed sloth   Bradypus       Pilosa herbi         <NA>
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
(data <- data %>%
  unite("diet", vore, suffix, sep = "", remove = TRUE)) %>%
  head()
```

    ##                         name      genus        order      diet
    ## 1                    Cheetah   Acinonyx    Carnivora carnivore
    ## 2                 Owl monkey      Aotus     Primates  omnivore
    ## 3            Mountain beaver Aplodontia     Rodentia herbivore
    ## 4 Greater short-tailed shrew    Blarina Soricomorpha  omnivore
    ## 5                        Cow        Bos Artiodactyla herbivore
    ## 6           Three-toed sloth   Bradypus       Pilosa herbivore
    ##   conservation sleep_total sleep_cycle awake brainwt  bodywt rem_ratio
    ## 1           lc        12.1          NA  11.9      NA  50.000        NA
    ## 2         <NA>        17.0          NA   7.0 0.01550   0.480 0.1058824
    ## 3           nt        14.4          NA   9.6      NA   1.350 0.1666667
    ## 4           lc        14.9   0.1333333   9.1 0.00029   0.019 0.1543624
    ## 5 domesticated         4.0   0.6666667  20.0 0.42300 600.000 0.1750000
    ## 6         <NA>        14.4   0.7666667   9.6      NA   3.850 0.1527778

spread()
--------

Now, let's say that we want to find out which *genera* fall under a type of diet. We can do this by "spreading" the *genera* against the diet type.

``` r
data %>%
  spread(diet, order) %>%
  select(name, ends_with("vore"))
```

    ##                              name       carnivore      herbivore
    ## 1                         Cheetah       Carnivora           <NA>
    ## 2                      Owl monkey            <NA>           <NA>
    ## 3                 Mountain beaver            <NA>       Rodentia
    ## 4      Greater short-tailed shrew            <NA>           <NA>
    ## 5                             Cow            <NA>   Artiodactyla
    ## 6                Three-toed sloth            <NA>         Pilosa
    ## 7               Northern fur seal       Carnivora           <NA>
    ## 8                             Dog       Carnivora           <NA>
    ## 9                        Roe deer            <NA>   Artiodactyla
    ## 10                           Goat            <NA>   Artiodactyla
    ## 11                     Guinea pig            <NA>       Rodentia
    ## 12                         Grivet            <NA>           <NA>
    ## 13                     Chinchilla            <NA>       Rodentia
    ## 14                Star-nosed mole            <NA>           <NA>
    ## 15      African giant pouched rat            <NA>           <NA>
    ## 16      Lesser short-tailed shrew            <NA>           <NA>
    ## 17           Long-nosed armadillo       Cingulata           <NA>
    ## 18                     Tree hyrax            <NA>     Hyracoidea
    ## 19         North American Opossum            <NA>           <NA>
    ## 20                 Asian elephant            <NA>    Proboscidea
    ## 21                  Big brown bat            <NA>           <NA>
    ## 22                          Horse            <NA> Perissodactyla
    ## 23                         Donkey            <NA> Perissodactyla
    ## 24              European hedgehog            <NA>           <NA>
    ## 25                   Patas monkey            <NA>           <NA>
    ## 26      Western american chipmunk            <NA>       Rodentia
    ## 27                   Domestic cat       Carnivora           <NA>
    ## 28                         Galago            <NA>           <NA>
    ## 29                        Giraffe            <NA>   Artiodactyla
    ## 30                    Pilot whale         Cetacea           <NA>
    ## 31                      Gray seal       Carnivora           <NA>
    ## 32                     Gray hyrax            <NA>     Hyracoidea
    ## 33                          Human            <NA>           <NA>
    ## 34                 Mongoose lemur            <NA>       Primates
    ## 35               African elephant            <NA>    Proboscidea
    ## 36           Thick-tailed opposum Didelphimorphia           <NA>
    ## 37                        Macaque            <NA>           <NA>
    ## 38               Mongolian gerbil            <NA>       Rodentia
    ## 39                 Golden hamster            <NA>       Rodentia
    ## 40                          Vole             <NA>       Rodentia
    ## 41                    House mouse            <NA>       Rodentia
    ## 42               Little brown bat            <NA>           <NA>
    ## 43           Round-tailed muskrat            <NA>       Rodentia
    ## 44                     Slow loris        Primates           <NA>
    ## 45                           Degu            <NA>       Rodentia
    ## 46     Northern grasshopper mouse        Rodentia           <NA>
    ## 47                         Rabbit            <NA>     Lagomorpha
    ## 48                          Sheep            <NA>   Artiodactyla
    ## 49                     Chimpanzee            <NA>           <NA>
    ## 50                          Tiger       Carnivora           <NA>
    ## 51                         Jaguar       Carnivora           <NA>
    ## 52                           Lion       Carnivora           <NA>
    ## 53                         Baboon            <NA>           <NA>
    ## 54                          Potto            <NA>           <NA>
    ## 55                   Caspian seal       Carnivora           <NA>
    ## 56                Common porpoise         Cetacea           <NA>
    ## 57                        Potoroo            <NA>  Diprotodontia
    ## 58                Giant armadillo            <NA>           <NA>
    ## 59                 Laboratory rat            <NA>       Rodentia
    ## 60          African striped mouse            <NA>           <NA>
    ## 61                Squirrel monkey            <NA>           <NA>
    ## 62          Eastern american mole            <NA>           <NA>
    ## 63                     Cotton rat            <NA>       Rodentia
    ## 64         Arctic ground squirrel            <NA>       Rodentia
    ## 65 Thirteen-lined ground squirrel            <NA>       Rodentia
    ## 66 Golden-mantled ground squirrel            <NA>       Rodentia
    ## 67                            Pig            <NA>           <NA>
    ## 68            Short-nosed echidna            <NA>           <NA>
    ## 69      Eastern american chipmunk            <NA>       Rodentia
    ## 70                Brazilian tapir            <NA> Perissodactyla
    ## 71                         Tenrec            <NA>           <NA>
    ## 72                     Tree shrew            <NA>           <NA>
    ## 73           Bottle-nosed dolphin         Cetacea           <NA>
    ## 74                          Genet       Carnivora           <NA>
    ## 75                     Arctic fox       Carnivora           <NA>
    ## 76                        Red fox       Carnivora           <NA>
    ##     insectivore        omnivore
    ## 1          <NA>            <NA>
    ## 2          <NA>        Primates
    ## 3          <NA>            <NA>
    ## 4          <NA>    Soricomorpha
    ## 5          <NA>            <NA>
    ## 6          <NA>            <NA>
    ## 7          <NA>            <NA>
    ## 8          <NA>            <NA>
    ## 9          <NA>            <NA>
    ## 10         <NA>            <NA>
    ## 11         <NA>            <NA>
    ## 12         <NA>        Primates
    ## 13         <NA>            <NA>
    ## 14         <NA>    Soricomorpha
    ## 15         <NA>        Rodentia
    ## 16         <NA>    Soricomorpha
    ## 17         <NA>            <NA>
    ## 18         <NA>            <NA>
    ## 19         <NA> Didelphimorphia
    ## 20         <NA>            <NA>
    ## 21   Chiroptera            <NA>
    ## 22         <NA>            <NA>
    ## 23         <NA>            <NA>
    ## 24         <NA>  Erinaceomorpha
    ## 25         <NA>        Primates
    ## 26         <NA>            <NA>
    ## 27         <NA>            <NA>
    ## 28         <NA>        Primates
    ## 29         <NA>            <NA>
    ## 30         <NA>            <NA>
    ## 31         <NA>            <NA>
    ## 32         <NA>            <NA>
    ## 33         <NA>        Primates
    ## 34         <NA>            <NA>
    ## 35         <NA>            <NA>
    ## 36         <NA>            <NA>
    ## 37         <NA>        Primates
    ## 38         <NA>            <NA>
    ## 39         <NA>            <NA>
    ## 40         <NA>            <NA>
    ## 41         <NA>            <NA>
    ## 42   Chiroptera            <NA>
    ## 43         <NA>            <NA>
    ## 44         <NA>            <NA>
    ## 45         <NA>            <NA>
    ## 46         <NA>            <NA>
    ## 47         <NA>            <NA>
    ## 48         <NA>            <NA>
    ## 49         <NA>        Primates
    ## 50         <NA>            <NA>
    ## 51         <NA>            <NA>
    ## 52         <NA>            <NA>
    ## 53         <NA>        Primates
    ## 54         <NA>        Primates
    ## 55         <NA>            <NA>
    ## 56         <NA>            <NA>
    ## 57         <NA>            <NA>
    ## 58    Cingulata            <NA>
    ## 59         <NA>            <NA>
    ## 60         <NA>        Rodentia
    ## 61         <NA>        Primates
    ## 62 Soricomorpha            <NA>
    ## 63         <NA>            <NA>
    ## 64         <NA>            <NA>
    ## 65         <NA>            <NA>
    ## 66         <NA>            <NA>
    ## 67         <NA>    Artiodactyla
    ## 68  Monotremata            <NA>
    ## 69         <NA>            <NA>
    ## 70         <NA>            <NA>
    ## 71         <NA>    Afrosoricida
    ## 72         <NA>      Scandentia
    ## 73         <NA>            <NA>
    ## 74         <NA>            <NA>
    ## 75         <NA>            <NA>
    ## 76         <NA>            <NA>

gather()
--------
