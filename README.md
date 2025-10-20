
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
    ## ✔ forcats   1.0.1     ✔ stringr   1.5.1
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

### Nhu Phan

#### Claim to check

> Of the nine Avengers we see on screen — Iron Man, Hulk, Captain
> America, Thor, Hawkeye, Black Widow, Scarlet Witch, Quicksilver and
> The Vision — every single one of them has died at least once in the
> course of their time Avenging in the comics. In fact, Hawkeye died
> twice!

``` r
library(tidyverse)

# Define the nine MCU Avengers mentioned in the article
mcu_avengers <- c(
  "Anthony Edward \"Tony\" Stark",   # Iron Man
  "Robert Bruce Banner",            # Hulk
  "Steven Rogers",                  # Captain America
  "Thor Odinson",                   # Thor
  "Clinton Francis Barton",                   # Hawkeye
  "Natalia Alianovna Romanova", # Black Widow
  "Wanda Maximoff",                 # Scarlet Witch
  "Pietro Maximoff",                # Quicksilver
  "Victor Shade (alias)"                          # Vision
)

mcu_deaths <- deaths %>%
  filter(Name.Alias %in% mcu_avengers & Death == "YES") %>%
  count(Name.Alias, name = "Times_Died") %>%
  arrange(desc(Times_Died))

mcu_deaths
```

    ## # A tibble: 9 × 2
    ##   Name.Alias                      Times_Died
    ##   <chr>                                <int>
    ## 1 "Clinton Francis Barton"                 4
    ## 2 "Thor Odinson"                           3
    ## 3 "Anthony Edward \"Tony\" Stark"          2
    ## 4 "Natalia Alianovna Romanova"             2
    ## 5 "Pietro Maximoff"                        2
    ## 6 "Robert Bruce Banner"                    2
    ## 7 "Steven Rogers"                          2
    ## 8 "Victor Shade (alias)"                   2
    ## 9 "Wanda Maximoff"                         2

#### Conclusion

Based on the data, every one of the nine MCU Avengers has died at least
once in the comics, confirming the main claim from the article. However,
the statement that only Hawkeye died twice is inaccurate. The dataset
shows that several Avengers — including Iron Man, Black Widow, Hulk,
Captain America, Vision, and others — have died more than once. Hawkeye
does have the highest number of deaths (four), but he is not the only
Avenger to experience multiple deaths. Therefore, while the overall idea
that the Avengers frequently die and return is true, the specific detail
about Hawkeye being unique in this regard is not supported by the data.

### Mahathi Reddy

> But you can only tempt death so many times. There’s a 2-in-3 chance
> that a member of the Avengers returned from their first stint in the
> afterlife, but only a 50 percent chance they recovered from a second
> or third death.

``` r
library(dplyr)

# Load the data
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)

# Check the structure of death and return columns
av %>%
  select(URL, Death1, Return1, Death2, Return2, Death3, Return3, Death4, Return4, Death5, Return5) %>%
  head(10)
```

    ##                                                        URL Death1 Return1
    ## 1            http://marvel.wikia.com/Henry_Pym_(Earth-616)    YES      NO
    ## 2       http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)    YES     YES
    ## 3        http://marvel.wikia.com/Anthony_Stark_(Earth-616)    YES     YES
    ## 4  http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)    YES     YES
    ## 5         http://marvel.wikia.com/Thor_Odinson_(Earth-616)    YES     YES
    ## 6        http://marvel.wikia.com/Richard_Jones_(Earth-616)     NO        
    ## 7        http://marvel.wikia.com/Steven_Rogers_(Earth-616)    YES     YES
    ## 8         http://marvel.wikia.com/Clint_Barton_(Earth-616)    YES     YES
    ## 9      http://marvel.wikia.com/Pietro_Maximoff_(Earth-616)    YES     YES
    ## 10      http://marvel.wikia.com/Wanda_Maximoff_(Earth-616)    YES     YES
    ##    Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                             
    ## 2                                                             
    ## 3                                                             
    ## 4                                                             
    ## 5     YES      NO                                             
    ## 6                                                             
    ## 7                                                             
    ## 8     YES     YES                                             
    ## 9                                                             
    ## 10

``` r
# Fact-check: Return rate from FIRST death
first_death_return <- av %>%
  filter(Death1 == "YES") %>%  # Only those who died at least once
  summarise(
    total_first_deaths = n(),
    returned_from_first = sum(Return1 == "YES", na.rm = TRUE),
    return_rate_first = mean(Return1 == "YES", na.rm = TRUE),
    return_percentage_first = mean(Return1 == "YES", na.rm = TRUE) * 100
  )

cat("FIRST DEATH:\n")
```

    ## FIRST DEATH:

``` r
print(first_death_return)
```

    ##   total_first_deaths returned_from_first return_rate_first
    ## 1                 69                  46         0.6666667
    ##   return_percentage_first
    ## 1                66.66667

``` r
cat("\nReturn rate:", round(first_death_return$return_rate_first, 3), 
    "(or", round(first_death_return$return_percentage_first, 1), "%)\n")
```

    ## 
    ## Return rate: 0.667 (or 66.7 %)

``` r
cat("This is approximately", round(first_death_return$return_rate_first * 3, 1), 
    "in 3, or", round(first_death_return$return_rate_first, 2), "\n\n")
```

    ## This is approximately 2 in 3, or 0.67

``` r
# Fact-check: Return rate from SECOND death
second_death_return <- av %>%
  filter(Death2 == "YES") %>%  # Only those who died a second time
  summarise(
    total_second_deaths = n(),
    returned_from_second = sum(Return2 == "YES", na.rm = TRUE),
    return_rate_second = mean(Return2 == "YES", na.rm = TRUE),
    return_percentage_second = mean(Return2 == "YES", na.rm = TRUE) * 100
  )

cat("SECOND DEATH:\n")
```

    ## SECOND DEATH:

``` r
print(second_death_return)
```

    ##   total_second_deaths returned_from_second return_rate_second
    ## 1                  16                    8                0.5
    ##   return_percentage_second
    ## 1                       50

``` r
cat("\nReturn rate:", round(second_death_return$return_rate_second, 3), 
    "(or", round(second_death_return$return_percentage_second, 1), "%)\n\n")
```

    ## 
    ## Return rate: 0.5 (or 50 %)

``` r
# Fact-check: Return rate from THIRD death
third_death_return <- av %>%
  filter(Death3 == "YES") %>%  # Only those who died a third time
  summarise(
    total_third_deaths = n(),
    returned_from_third = sum(Return3 == "YES", na.rm = TRUE),
    return_rate_third = mean(Return3 == "YES", na.rm = TRUE),
    return_percentage_third = mean(Return3 == "YES", na.rm = TRUE) * 100
  )

cat("THIRD DEATH:\n")
```

    ## THIRD DEATH:

``` r
print(third_death_return)
```

    ##   total_third_deaths returned_from_third return_rate_third
    ## 1                  2                   1               0.5
    ##   return_percentage_third
    ## 1                      50

``` r
cat("\nReturn rate:", round(third_death_return$return_rate_third, 3), 
    "(or", round(third_death_return$return_percentage_third, 1), "%)\n\n")
```

    ## 
    ## Return rate: 0.5 (or 50 %)

``` r
# Combined second and third death return rate
second_and_third_combined <- av %>%
  filter(Death2 == "YES" | Death3 == "YES") %>%
  mutate(
    died_second_or_third = (Death2 == "YES") + (Death3 == "YES"),
    returned_second_or_third = (Return2 == "YES") + (Return3 == "YES")
  ) %>%
  summarise(
    total_deaths_2_or_3 = sum(died_second_or_third, na.rm = TRUE),
    total_returns_2_or_3 = sum(returned_second_or_third, na.rm = TRUE),
    combined_return_rate = total_returns_2_or_3 / total_deaths_2_or_3,
    combined_return_percentage = (total_returns_2_or_3 / total_deaths_2_or_3) * 100
  )

cat("SECOND OR THIRD DEATH COMBINED:\n")
```

    ## SECOND OR THIRD DEATH COMBINED:

``` r
print(second_and_third_combined)
```

    ##   total_deaths_2_or_3 total_returns_2_or_3 combined_return_rate
    ## 1                  18                    9                  0.5
    ##   combined_return_percentage
    ## 1                         50

``` r
cat("\nCombined return rate:", round(second_and_third_combined$combined_return_rate, 3), 
    "(or", round(second_and_third_combined$combined_return_percentage, 1), "%)\n\n")
```

    ## 
    ## Combined return rate: 0.5 (or 50 %)

``` r
# Summary comparison table
summary_table <- tibble(
  Death_Number = c("First Death", "Second Death", "Third Death", "Second or Third Combined"),
  Total_Deaths = c(first_death_return$total_first_deaths,
                   second_death_return$total_second_deaths,
                   third_death_return$total_third_deaths,
                   second_and_third_combined$total_deaths_2_or_3),
  Returns = c(first_death_return$returned_from_first,
              second_death_return$returned_from_second,
              third_death_return$returned_from_third,
              second_and_third_combined$total_returns_2_or_3),
  Return_Rate = c(first_death_return$return_rate_first,
                  second_death_return$return_rate_second,
                  third_death_return$return_rate_third,
                  second_and_third_combined$combined_return_rate),
  Return_Percentage = c(first_death_return$return_percentage_first,
                        second_death_return$return_percentage_second,
                        third_death_return$return_percentage_third,
                        second_and_third_combined$combined_return_percentage)
)

cat("SUMMARY TABLE:\n")
```

    ## SUMMARY TABLE:

``` r
print(summary_table)
```

    ## # A tibble: 4 × 5
    ##   Death_Number             Total_Deaths Returns Return_Rate Return_Percentage
    ##   <chr>                           <int>   <int>       <dbl>             <dbl>
    ## 1 First Death                        69      46       0.667              66.7
    ## 2 Second Death                       16       8       0.5                50  
    ## 3 Third Death                         2       1       0.5                50  
    ## 4 Second or Third Combined           18       9       0.5                50

``` r
# Verify the "2-in-3" claim (which is approximately 0.667 or 66.7%)
cat("\n=== VERIFICATION ===\n")
```

    ## 
    ## === VERIFICATION ===

``` r
cat("Claim: 2-in-3 chance (66.7%) of return from first death\n")
```

    ## Claim: 2-in-3 chance (66.7%) of return from first death

``` r
cat("Actual:", round(first_death_return$return_percentage_first, 1), "%\n")
```

    ## Actual: 66.7 %

``` r
cat("Difference:", round(first_death_return$return_percentage_first - 66.7, 1), "percentage points\n\n")
```

    ## Difference: 0 percentage points

``` r
cat("Claim: 50% chance of return from second or third death\n")
```

    ## Claim: 50% chance of return from second or third death

``` r
cat("Actual:", round(second_and_third_combined$combined_return_percentage, 1), "%\n")
```

    ## Actual: 50 %

``` r
cat
```

    ## function (..., file = "", sep = " ", fill = FALSE, labels = NULL, 
    ##     append = FALSE) 
    ## {
    ##     if (is.character(file)) 
    ##         if (file == "") 
    ##             file <- stdout()
    ##         else if (startsWith(file, "|")) {
    ##             file <- pipe(substring(file, 2L), "w")
    ##             on.exit(close(file))
    ##         }
    ##         else {
    ##             file <- file(file, ifelse(append, "a", "w"))
    ##             on.exit(close(file))
    ##         }
    ##     .Internal(cat(list(...), file, sep, fill, labels, append))
    ## }
    ## <bytecode: 0x14514b1c8>
    ## <environment: namespace:base>

There is no difference between the claim and the returned value. This
means that there is a 2 in 3 chance that a member of the Avengers
returned from their first stint in the afterlife, but only a 50 percent
chance they recovered from a second or third death.
