# Pamgrans and Bipams
Mara Averick  
`r Sys.Date()`  




```r
## load packages
library(tidyverse)
library(tidytext)
library(here)
```

Let's read in the Pam-bigram data (which you can access for your own amusement [here](https://gist.github.com/batpigandme/7fd3266451392d0310ebefd0bc472c0f)).


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
##               bigram  freq speaker
##                <chr> <int>   <chr>
## 1          yeah blue     1     PAM
## 2          hot asian     1     PAM
## 3       frickin edie     1     PAM
## 4    kill decepticon     1     PAM
## 5           aw maaan     1     PAM
## 6        crazy pants     1     PAM
## 7  penis ensmallment     1     PAM
## 8       fawty shawty     1     PAM
## 9       whitey crane     1     PAM
## 10        primo shit     1     PAM
## 11 wiped reformatted     1     PAM
## 12          moto san     1     PAM
## 13           sold em     1     PAM
## 14        ya krieger     1     PAM
## 15      killing cops     1     PAM
```

## Tables of Pam

Since pretty much every pair of words Pam utters is priceless, let's check out some of the different ways you can display data frames as [tables in R Markdown](http://rmarkdown.rstudio.com/lesson-7.html). 

### `kable`

First, we might as well make use of the `knitr::kable` function, since we're knitting this document anyhow. Note that the chunk option `results='asis'` needs to be set so that our output isn't further processed by knitr when we pull our document together.[^1]


```r
library(knitr)
kable(sample_n(pam_bigrams, 15), caption = "kable pamgrams")
```



Table: kable pamgrams

bigram             freq  speaker 
----------------  -----  --------
maximum voltage       1  PAM     
buck fifty            1  PAM     
meal snacks           1  PAM     
fister roboto         1  PAM     
dial tone             1  PAM     
murder mystery        1  PAM     
wooden stakes         1  PAM     
god lana              1  PAM     
skinny ass            2  PAM     
cliburn knock         1  PAM     
easy spirit           1  PAM     
peak oil              1  PAM     
stayin alive          1  PAM     
black guys            1  PAM     
real bank             1  PAM     

Ok, we've got bold headers, and lines between rows -- basically, things are looking a little more _table-y_ than the did before. The [`kable()`](https://www.rdocumentation.org/packages/knitr/versions/1.15.1/topics/kable) function can take other formatting arguments, but we're just gonna leave it at the defaults here.

### `pander`

[`pander`](https://rapporter.github.io/pander/) is another R Pandoc writer. It supports [four Pandoc formats](https://rapporter.github.io/pander/#markdown-tables) for tables (which can be specified using `pandoc.table`[^2]), and you may or may not want to add some information about that in your frontmatter/YAML. You can also make more adjustments to your table formatting using [`panderOptions()`](https://rapporter.github.io/pander/#pander-options). 


```r
library(pander)
## multiline table by default
pandoc.table(sample_n(pam_bigrams, 5))
```

```
## 
## ------------------------------------
##       bigram         freq   speaker 
## ------------------- ------ ---------
## commercial driver's   1       PAM   
## 
##    ingrown hairs      1       PAM   
## 
##    cyril bailed       1       PAM   
## 
##     tits whaaat       2       PAM   
## 
##   holy dickballs      1       PAM   
## ------------------------------------
```

```r
## simple tables
pandoc.table(sample_n(pam_bigrams, 5), style = "simple")
```

```
## 
## 
##      bigram       freq   speaker 
## ---------------- ------ ---------
##  isis intranet     1       PAM   
##   bell bottoms     1       PAM   
## hollywood honcho   1       PAM   
##    head smash      1       PAM   
##     hey edie       1       PAM
```


```r
## grid tables
pandoc.table(sample_n(pam_bigrams, 5), style = "grid")
```

```
## 
## 
## +-----------------+--------+-----------+
## |     bigram      |  freq  |  speaker  |
## +=================+========+===========+
## | grain alcohol's |   1    |    PAM    |
## +-----------------+--------+-----------+
## |    o.s cyril    |   1    |    PAM    |
## +-----------------+--------+-----------+
## |    car horn     |   1    |    PAM    |
## +-----------------+--------+-----------+
## |  lana's pissed  |   1    |    PAM    |
## +-----------------+--------+-----------+
## |    neck nuts    |   1    |    PAM    |
## +-----------------+--------+-----------+
```


```r
## rmarkdown tables (aka pipe table format)
pandoc.table(sample_n(pam_bigrams, 5), style = "rmarkdown")
```

```
## 
## 
## |     bigram      |  freq  |  speaker  |
## |:---------------:|:------:|:---------:|
## |    black eye    |   1    |    PAM    |
## |  sobbing holy   |   1    |    PAM    |
## | rural wisconsin |   1    |    PAM    |
## |   baby seamus   |   2    |    PAM    |
## |    low blood    |   1    |    PAM    |
```

There are a bunch of other options you can play around with as well, such as [captions](https://rapporter.github.io/pander/#caption), [cell highlighting](https://rapporter.github.io/pander/#highlight-cells), [cell alignment](https://rapporter.github.io/pander/#cell-alignment), and [width](https://rapporter.github.io/pander/#table-and-cell-width).

### `xtable`

The [`xtable`](http://crantastic.org/packages/xtable) package has been around for a while, and it has a _ton_ of formatting options for both LaTeX and HTML output. (Not to be confused with a _tonne_ of options, which -- how does one even measure formatting options in kilos?) The output defaults to latex, and, as with `kable`, we'll need to be sure to include `results="asis"` in our chunk options.


```r
library(xtable)
xpam <- xtable(sample_n(pam_bigrams, 15))
print(xpam, type="html")
```

<!-- html table generated in R 3.4.0 by xtable 1.8-2 package -->
<!-- Sun May  7 09:40:59 2017 -->
<table border=1>
<tr> <th>  </th> <th> bigram </th> <th> freq </th> <th> speaker </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> sharknoid 3 </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 2 </td> <td> start enjoying </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 3 </td> <td> rosemary mint </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 4 </td> <td> step ahead </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 5 </td> <td> brown mica </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 6 </td> <td> shit's sake </td> <td align="right">   2 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 7 </td> <td> switchblade tracheotomygivin </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 8 </td> <td> puddin ooh </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 9 </td> <td> jack dick </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 10 </td> <td> that're strictly </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 11 </td> <td> heart beating </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 12 </td> <td> fawty shawty </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 13 </td> <td> zitto testa </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 14 </td> <td> wee baby </td> <td align="right">   2 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 15 </td> <td> date dates </td> <td align="right">   1 </td> <td> PAM </td> </tr>
   </table>

Yeah, so, the default doesn't look so awesome right now. However, this is really just an HTML table, so it can be easily styled as such (in addition to using the `xtable` parameters).

### `huxtable`

Like `xtable`, [`huxtable`](https://hughjonesd.github.io/huxtable/) deals in both LaTeX and HTML, and has a ton of [properties](https://hughjonesd.github.io/huxtable/huxtable.html) you can adjust to your liking (you can even use the `magrittr`-style pipe operator, `%>%`, to do so). The number format defaults to two decimal places, so we'll set that manually here. In order for the column names to show up in your huxtable, they need to be added to the data frame itself. If you're working in R Markdown, it _will_ look like they show up twice, but that shouldn't be the case once you've knit your document.


```r
library(huxtable)
hpam <- hux(sample_n(pam_bigrams, 15)) %>%
  huxtable::add_colnames()
number_format(hpam) <- 0
hpam
```

<!--html_preserve--><table class="huxtable" style="border-collapse: collapse; width: 50%; margin-left: auto; margin-right: auto;">
<col style="width: NA;"><col style="width: NA;"><col style="width: NA;"><tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">bigram</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">freq</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">speaker</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">entire submarine</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">steering wheel's</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">owww jesus</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">name's jermaaaine</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">ahem wildly</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">innocent migrant</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">buyer lined</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">fence post</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">dammit dude</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">breakup uh</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">hip ahem</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">kill decepticon</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">pure ethanol</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">princess hang</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">consenting adult</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
</table>
<!--/html_preserve-->

Though it's a bit clunky for such simple output, there are [more options](https://hughjonesd.github.io/huxtable/huxtable.html#more-formatting), such as conditional formatting, as well as some very fine-tuned controls that may make `huxtable` a good choice for you.

### `printr`

[Yihui Xie](https://yihui.name), the author of `knitr`, and a bevy of [other useful R packages](https://github.com/yihui/) (including most of the ones ending in "-down"), also created the [`printr`](https://yihui.name/printr/) package as a "companion" to knitr. And, as far as data frames, tibbles, and the like go, the output will look the same as it does using `kable()` without `printr` attached. However, it _will_ make a difference in terms of the way things like matrices, or lists of vignettes, and datasets are displayed.


```r
library(printr)
kable(sample_n(pam_bigrams, 15), caption = "printr pamgrams")
```



Table: printr pamgrams

bigram               freq  speaker 
------------------  -----  --------
hypnotized people       1  PAM     
fricking head's         1  PAM     
worst idea              1  PAM     
dip nuts                1  PAM     
mm kay                  1  PAM     
ow hey                  1  PAM     
whitey's bullshit       1  PAM     
feel sexy               1  PAM     
frickin zod             1  PAM     
kidnapper's neck        1  PAM     
kill key                1  PAM     
lot huh                 1  PAM     
scrote licks            1  PAM     
follow kenny            1  PAM     
cold pony               1  PAM     


```r
vignette(package = "printr")
```



Table: Vignettes in printr

Item     Title                                                
-------  -----------------------------------------------------
printr   An Introduction to the printr Package (source, html) 



[^1]: See RStudio's [R Markdown -- Tables](http://rmarkdown.rstudio.com/lesson-7.html) documentation for more on this.
[^2]: By default, it will use [`mulitiline` format](http://pandoc.org/MANUAL.html#multiline-tables), but there's not much in the way of visuals for that with these bigrams.
