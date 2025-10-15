
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

``` r
str(av)
```

    ## 'data.frame':    173 obs. of  21 variables:
    ##  $ URL                        : chr  "http://marvel.wikia.com/Henry_Pym_(Earth-616)" "http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)" "http://marvel.wikia.com/Anthony_Stark_(Earth-616)" "http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)" ...
    ##  $ Name.Alias                 : chr  "Henry Jonathan \"Hank\" Pym" "Janet van Dyne" "Anthony Edward \"Tony\" Stark" "Robert Bruce Banner" ...
    ##  $ Appearances                : int  1269 1165 3068 2089 2402 612 3458 1456 769 1214 ...
    ##  $ Current.                   : chr  "YES" "YES" "YES" "YES" ...
    ##  $ Gender                     : chr  "MALE" "FEMALE" "MALE" "MALE" ...
    ##  $ Probationary.Introl        : chr  "" "" "" "" ...
    ##  $ Full.Reserve.Avengers.Intro: chr  "Sep-63" "Sep-63" "Sep-63" "Sep-63" ...
    ##  $ Year                       : int  1963 1963 1963 1963 1963 1963 1964 1965 1965 1965 ...
    ##  $ Years.since.joining        : int  52 52 52 52 52 52 51 50 50 50 ...
    ##  $ Honorary                   : chr  "Full" "Full" "Full" "Full" ...
    ##  $ Death1                     : chr  "YES" "YES" "YES" "YES" ...
    ##  $ Return1                    : chr  "NO" "YES" "YES" "YES" ...
    ##  $ Death2                     : chr  "" "" "" "" ...
    ##  $ Return2                    : chr  "" "" "" "" ...
    ##  $ Death3                     : chr  "" "" "" "" ...
    ##  $ Return3                    : chr  "" "" "" "" ...
    ##  $ Death4                     : chr  "" "" "" "" ...
    ##  $ Return4                    : chr  "" "" "" "" ...
    ##  $ Death5                     : chr  "" "" "" "" ...
    ##  $ Return5                    : chr  "" "" "" "" ...
    ##  $ Notes                      : chr  "Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. " "Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered" "Death: \"Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'"| __truncated__ "Dies in Ghosts of the Future arc. However \"he had actually used a hidden Pantheon base to survive\"" ...

## Death Mutate

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.2     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
deaths <- av %>%
  pivot_longer(
    cols = Death1:Death5,
    names_to = "Time",
    values_to = "Death"
  ) %>%
  mutate(Time = parse_number(Time)) %>%
  filter(Death != "")
head(deaths, 10)
```

    ## # A tibble: 10 × 14
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  3 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  4 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  5 http://marvel.wik… "Anthony …        3068 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Anthony …        3068 YES      MALE   ""                 
    ##  7 http://marvel.wik… "Robert B…        2089 YES      MALE   ""                 
    ##  8 http://marvel.wik… "Robert B…        2089 YES      MALE   ""                 
    ##  9 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ## 10 http://marvel.wik… "Thor Odi…        2402 YES      MALE   ""                 
    ## # ℹ 8 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return5 <chr>, Notes <chr>,
    ## #   Time <dbl>, Death <chr>

``` r
str(deaths)
```

    ## tibble [282 × 14] (S3: tbl_df/tbl/data.frame)
    ##  $ URL                        : chr [1:282] "http://marvel.wikia.com/Henry_Pym_(Earth-616)" "http://marvel.wikia.com/Henry_Pym_(Earth-616)" "http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)" "http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)" ...
    ##  $ Name.Alias                 : chr [1:282] "Henry Jonathan \"Hank\" Pym" "Henry Jonathan \"Hank\" Pym" "Janet van Dyne" "Janet van Dyne" ...
    ##  $ Appearances                : int [1:282] 1269 1269 1165 1165 3068 3068 2089 2089 2402 2402 ...
    ##  $ Current.                   : chr [1:282] "YES" "YES" "YES" "YES" ...
    ##  $ Gender                     : chr [1:282] "MALE" "MALE" "FEMALE" "FEMALE" ...
    ##  $ Probationary.Introl        : chr [1:282] "" "" "" "" ...
    ##  $ Full.Reserve.Avengers.Intro: chr [1:282] "Sep-63" "Sep-63" "Sep-63" "Sep-63" ...
    ##  $ Year                       : int [1:282] 1963 1963 1963 1963 1963 1963 1963 1963 1963 1963 ...
    ##  $ Years.since.joining        : int [1:282] 52 52 52 52 52 52 52 52 52 52 ...
    ##  $ Honorary                   : chr [1:282] "Full" "Full" "Full" "Full" ...
    ##  $ Return5                    : chr [1:282] "" "" "" "" ...
    ##  $ Notes                      : chr [1:282] "Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. " "Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. " "Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered" "Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered" ...
    ##  $ Time                       : num [1:282] 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ Death                      : chr [1:282] "YES" "NO" "YES" "YES" ...

## Returns Mutate

``` r
library(tidyverse)

returns <- av %>%
  pivot_longer(
    cols = Return1:Return5,
    names_to = "Time",
    values_to = "Return"
  ) %>%
  mutate(Time = parse_number(Time)) %>%
  filter(Return != "")

head(returns)
```

    ## # A tibble: 6 × 14
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## 3 http://marvel.wiki… "Anthony …        3068 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Robert B…        2089 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Thor Odi…        2402 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Thor Odi…        2402 YES      MALE   ""                 
    ## # ℹ 8 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Death1 <chr>, Notes <chr>,
    ## #   Time <dbl>, Return <chr>

## Average Deaths

``` r
total_avengers <- nrow(av)
total_deaths <- deaths %>% 
  filter(Death == "YES") %>% 
  nrow()

avg_deaths <- total_deaths / total_avengers
print(avg_deaths)
```

    ## [1] 0.8381503

## Individually

### Harrison Voss

#### FiveThirtyEight statement

> Out of 173 listed Avengers, my analysis found that 69 had died at
> least one time after they joined the team.5 That’s about 40 percent of
> all people who have ever signed on to the team.

#### Code

``` r
total_avengers <- nrow(av)

avengers_who_died <- deaths |>
  filter(Death == "YES") |>
  distinct(Name.Alias) |>
  nrow()


print(avengers_who_died)
```

    ## [1] 64

``` r
percent_died <- avengers_who_died / total_avengers

print(percent_died* 100)
```

    ## [1] 36.99422

#### Answer

It seems that the number of people who died was 64, not 69, and that the
percentage is 37% not 40%.

This is not a very big discrepancy, but my findings do differ from the
claim in the analysis.
