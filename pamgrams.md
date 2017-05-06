# Pamgrans and Bipams
Mara Averick  
`r Sys.Date()`  




```r
## load packages
library(tidyverse)
library(tidytext)
library(here)
```

Let's read in the Pam-bigram data.


```r
## read csv file
pam_bigrams <- read_csv("data/pam_bigrams.csv")
```

Now we'll use the `sample_n()` function from `dplyr` to get 15 (though you could use any number) observations/rows from our pamgram data. Note that the `freq` variable contains the number of times a given bigram occurred in the "PAM" corpus (which, as of this writing, includes all of Archer, seasons 1 - 7).


```r
sample_n(pam_bigrams, 15)
```

```
## # A tibble: 15 × 3
##              bigram  freq speaker
##               <chr> <int>   <chr>
## 1    cocaine puddin     3     PAM
## 2        fire drill     1     PAM
## 3      dick holster     1     PAM
## 4     party krieger     1     PAM
## 5        ow goddamn     2     PAM
## 6          pay bums     1     PAM
## 7        gal's love     1     PAM
## 8     dance partner     1     PAM
## 9     black mexican     1     PAM
## 10     skinny bitch     1     PAM
## 11 insurance policy     1     PAM
## 12         wee baby     2     PAM
## 13  holy shitsnacks    15     PAM
## 14      truck noise     1     PAM
## 15     homemade gin     1     PAM
```

