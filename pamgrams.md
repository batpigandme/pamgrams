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
##                  bigram  freq speaker
##                   <chr> <int>   <chr>
## 1          stayin alive     1     PAM
## 2           word choice     1     PAM
## 3          han fastolfe     1     PAM
## 4         krieger sells     1     PAM
## 5              ham cozy     1     PAM
## 6         damn sputters     1     PAM
## 7           ahem wildly     1     PAM
## 8         kentucky moon     1     PAM
## 9      gehrig's disease     1     PAM
## 10              aw damn     1     PAM
## 11            lunch pam     1     PAM
## 12             red hook     2     PAM
## 13 ultimate aphrodisiac     1     PAM
## 14         archer joint     1     PAM
## 15           horn honks     1     PAM
```

## Tables of Pam

Since pretty much every pair of words Pam utters is priceless, let's check out some of the different ways you can display data frames as [tables in R Markdown](http://rmarkdown.rstudio.com/lesson-7.html). If you're looking at this in markdown (`.md` file), what you see may not match the descriptions. This makes sense, and is worth paying attention to for things like GitHub's README.md files, and the like.

### `kable`

First, we might as well make use of the `knitr::kable` function, since we're knitting this document anyhow. Note that the chunk option `results='asis'` needs to be set so that our output isn't further processed by knitr when we pull our document together.[^1]


```r
library(knitr)
kable(sample_n(pam_bigrams, 15), caption = "kable pamgrams")
```



Table: kable pamgrams

bigram                  freq  speaker 
---------------------  -----  --------
claw monday                1  PAM     
brake line's               1  PAM     
hoh lee                    1  PAM     
god cyril                  1  PAM     
dunno what're              1  PAM     
name's cher                1  PAM     
wildly inappropriate       1  PAM     
freakin beers              1  PAM     
scatterbrain jane          1  PAM     
baby shower                1  PAM     
elevator supposed          1  PAM     
5 discount                 1  PAM     
pennies crammed            1  PAM     
shit kicked                1  PAM     
agent kane                 1  PAM     

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
##  shitsnackin whore    1       PAM   
## 
##   shoulda looked      1       PAM   
## 
##   crónico gracias     1       PAM   
## 
##   japanese onsen      1       PAM   
## 
## literal bellyaching   1       PAM   
## ------------------------------------
```

```r
## simple tables
pandoc.table(sample_n(pam_bigrams, 5), style = "simple")
```

```
## 
## 
##     bigram      freq   speaker 
## -------------- ------ ---------
##  giant pussy     1       PAM   
##    dick dad      1       PAM   
## arigato fellas   1       PAM   
##    huh yum       1       PAM   
##  black person    1       PAM
```


```r
## grid tables
pandoc.table(sample_n(pam_bigrams, 5), style = "grid")
```

```
## 
## 
## +---------------+--------+-----------+
## |    bigram     |  freq  |  speaker  |
## +===============+========+===========+
## |    teen 40    |   1    |    PAM    |
## +---------------+--------+-----------+
## |  nazi clone   |   1    |    PAM    |
## +---------------+--------+-----------+
## | hard watching |   1    |    PAM    |
## +---------------+--------+-----------+
## |  bucks worth  |   1    |    PAM    |
## +---------------+--------+-----------+
## |  laughs yeah  |   1    |    PAM    |
## +---------------+--------+-----------+
```


```r
## rmarkdown tables (aka pipe table format)
pandoc.table(sample_n(pam_bigrams, 5), style = "rmarkdown")
```

```
## 
## 
## |      bigram       |  freq  |  speaker  |
## |:-----------------:|:------:|:---------:|
## | dastardly dagoes  |   1    |    PAM    |
## |   coke lickbag    |   1    |    PAM    |
## |    forty ounce    |   1    |    PAM    |
## | shitsnacks lookit |   1    |    PAM    |
## |   clingy shirt    |   1    |    PAM    |
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
<!-- Sun May  7 09:52:13 2017 -->
<table border=1>
<tr> <th>  </th> <th> bigram </th> <th> freq </th> <th> speaker </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> baby a.j </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 2 </td> <td> telephone ringing </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 3 </td> <td> grrgh grrgh </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 4 </td> <td> ar randy </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 5 </td> <td> easter dinner </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 6 </td> <td> 2 sharknoid </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 7 </td> <td> blow jobs </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 8 </td> <td> real knee </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 9 </td> <td> coke snacks </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 10 </td> <td> worst frickin </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 11 </td> <td> hai moto </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 12 </td> <td> super drunk </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 13 </td> <td> lincoln log </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 14 </td> <td> date dates </td> <td align="right">   1 </td> <td> PAM </td> </tr>
  <tr> <td align="right"> 15 </td> <td> fat smelly </td> <td align="right">   1 </td> <td> PAM </td> </tr>
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
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">shitsnacks krieger's</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">field agent</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">yom yom</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">stay empty</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">happenin sunday</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">ungh ungh</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">14</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">black tie</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">hip ahem</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">rebel flag</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">whitey's bullshit</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">frstrrrdkerrrrrt erperperrrrrn</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">cockblock archer</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">game called</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">hippie shirt</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">1</td>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">PAM</td>
</tr>
<tr>
  <td  style="vertical-align: top; text-align: left; white-space: nowrap; border-width:0px 0px 0px 0px; border-style: solid; border-top-color: NA;  border-right-color: NA;  border-bottom-color: NA;  border-left-color: NA; padding: 4pt 4pt 4pt 4pt; ">dial tone</td>
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

bigram                  freq  speaker 
---------------------  -----  --------
gonna drop                 1  PAM     
geezy petes                1  PAM     
frickin brake              1  PAM     
dude helping               1  PAM     
joe frazier's              1  PAM     
therapist's cock           1  PAM     
wildly inappropriate       1  PAM     
pam wait                   1  PAM     
um psst                    1  PAM     
explosion sound            1  PAM     
blintzen nice              1  PAM     
lovable driver             1  PAM     
ol whitey                  1  PAM     
starting rumors            1  PAM     
greenbay wisconsin         1  PAM     


```r
vignette(package = "printr")
```



Table: Vignettes in printr

Item     Title                                                
-------  -----------------------------------------------------
printr   An Introduction to the printr Package (source, html) 
---

```r
sessionInfo()
```

```
## R version 3.4.0 (2017-04-21)
## Platform: x86_64-apple-darwin15.6.0 (64-bit)
## Running under: macOS Sierra 10.12.4
## 
## Matrix products: default
## BLAS: /Library/Frameworks/R.framework/Versions/3.4/Resources/lib/libRblas.0.dylib
## LAPACK: /Library/Frameworks/R.framework/Versions/3.4/Resources/lib/libRlapack.dylib
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
##  [1] printr_0.0.6         huxtable_0.2.2       xtable_1.8-2        
##  [4] pander_0.6.0         knitr_1.15.20        here_0.0-6          
##  [7] tidytext_0.1.2.900   forcats_0.2.0        stringr_1.2.0       
## [10] dplyr_0.5.0          purrr_0.2.2          readr_1.1.0         
## [13] tidyr_0.6.2          tibble_1.3.0         ggplot2_2.2.1       
## [16] tidyverse_1.1.1.9000
## 
## loaded via a namespace (and not attached):
##  [1] reshape2_1.4.2.9000   haven_1.0.0           lattice_0.20-35      
##  [4] colorspace_1.3-2      htmltools_0.3.6       SnowballC_0.5.1      
##  [7] yaml_2.1.14           foreign_0.8-68        DBI_0.6-1            
## [10] modelr_0.1.0          readxl_1.0.0          plyr_1.8.4           
## [13] munsell_0.4.3         gtable_0.2.0          cellranger_1.1.0     
## [16] rvest_0.3.2           psych_1.7.5           evaluate_0.10        
## [19] parallel_3.4.0        highr_0.6             broom_0.4.2          
## [22] tokenizers_0.1.4.9000 Rcpp_0.12.10          scales_0.4.1         
## [25] backports_1.0.5       jsonlite_1.4          mnormt_1.5-5         
## [28] hms_0.3               digest_0.6.12         stringi_1.1.5        
## [31] grid_3.4.0            rprojroot_1.2         tools_3.4.0          
## [34] magrittr_1.5          lazyeval_0.2.0.9000   janeaustenr_0.1.4    
## [37] Matrix_1.2-10         xml2_1.1.1            lubridate_1.6.0      
## [40] assertthat_0.2.0      rmarkdown_1.5.9000    httr_1.2.1.9000      
## [43] rstudioapi_0.6        R6_2.2.0              nlme_3.1-131         
## [46] compiler_3.4.0
```



[^1]: See RStudio's [R Markdown -- Tables](http://rmarkdown.rstudio.com/lesson-7.html) documentation for more on this.
[^2]: By default, it will use [`mulitiline` format](http://pandoc.org/MANUAL.html#multiline-tables), but there's not much in the way of visuals for that with these bigrams.
