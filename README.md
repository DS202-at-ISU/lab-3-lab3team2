
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

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyr)
library(readr)
library(tidyverse)
```

    ## Warning: package 'ggplot2' was built under R version 4.2.3

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ forcats   1.0.0     ✔ purrr     1.0.1
    ## ✔ ggplot2   3.5.1     ✔ stringr   1.5.0
    ## ✔ lubridate 1.9.2     ✔ tibble    3.1.8

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the ]8;;http://conflicted.r-lib.org/conflicted package]8;; to force all conflicts to become errors

``` r
deaths <- av %>%
  pivot_longer(cols = matches("Death[1-5]"),
               names_to = "Time",
               values_to = "Death") %>%
  mutate(Time = parse_number(Time),
         Death = tolower(Death)) %>%
  replace_na(list(Death = ""))

head(deaths)
```

    ## # A tibble: 6 × 18
    ##   URL       Name.…¹ Appea…² Curre…³ Gender Proba…⁴ Full.…⁵  Year Years…⁶ Honor…⁷
    ##   <chr>     <chr>     <int> <chr>   <chr>  <chr>   <chr>   <int>   <int> <chr>  
    ## 1 http://m… "Henry…    1269 YES     MALE   ""      Sep-63   1963      52 Full   
    ## 2 http://m… "Henry…    1269 YES     MALE   ""      Sep-63   1963      52 Full   
    ## 3 http://m… "Henry…    1269 YES     MALE   ""      Sep-63   1963      52 Full   
    ## 4 http://m… "Henry…    1269 YES     MALE   ""      Sep-63   1963      52 Full   
    ## 5 http://m… "Henry…    1269 YES     MALE   ""      Sep-63   1963      52 Full   
    ## 6 http://m… "Janet…    1165 YES     FEMALE ""      Sep-63   1963      52 Full   
    ## # … with 8 more variables: Return1 <chr>, Return2 <chr>, Return3 <chr>,
    ## #   Return4 <chr>, Return5 <chr>, Notes <chr>, Time <dbl>, Death <chr>, and
    ## #   abbreviated variable names ¹​Name.Alias, ²​Appearances, ³​Current.,
    ## #   ⁴​Probationary.Introl, ⁵​Full.Reserve.Avengers.Intro, ⁶​Years.since.joining,
    ## #   ⁷​Honorary

Similarly, deal with the returns of characters.

``` r
library(tidyverse)
returns <- av %>%
  pivot_longer(cols = starts_with("Return"), names_to = "Time", values_to = "Return") %>%
  mutate(Time = parse_number(Time))

head(returns)
```

    ## # A tibble: 6 × 18
    ##   URL       Name.…¹ Appea…² Curre…³ Gender Proba…⁴ Full.…⁵  Year Years…⁶ Honor…⁷
    ##   <chr>     <chr>     <int> <chr>   <chr>  <chr>   <chr>   <int>   <int> <chr>  
    ## 1 http://m… "Henry…    1269 YES     MALE   ""      Sep-63   1963      52 Full   
    ## 2 http://m… "Henry…    1269 YES     MALE   ""      Sep-63   1963      52 Full   
    ## 3 http://m… "Henry…    1269 YES     MALE   ""      Sep-63   1963      52 Full   
    ## 4 http://m… "Henry…    1269 YES     MALE   ""      Sep-63   1963      52 Full   
    ## 5 http://m… "Henry…    1269 YES     MALE   ""      Sep-63   1963      52 Full   
    ## 6 http://m… "Janet…    1165 YES     FEMALE ""      Sep-63   1963      52 Full   
    ## # … with 8 more variables: Death1 <chr>, Death2 <chr>, Death3 <chr>,
    ## #   Death4 <chr>, Death5 <chr>, Notes <chr>, Time <dbl>, Return <chr>, and
    ## #   abbreviated variable names ¹​Name.Alias, ²​Appearances, ³​Current.,
    ## #   ⁴​Probationary.Introl, ⁵​Full.Reserve.Avengers.Intro, ⁶​Years.since.joining,
    ## #   ⁷​Honorary

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
avg_deaths <- deaths %>%
  mutate(DeathCount = ifelse(Death == "yes", 1, 0)) %>%  # Convert "yes" to 1, others to 0
  group_by(Name.Alias) %>%                               # Group by Avenger name
  summarise(AvgDeaths = sum(DeathCount)) %>%             # Sum Deaths per Avenger
  summarise(AverageDeaths = mean(AvgDeaths, na.rm = TRUE))  # Calculate overall average

avg_deaths
```

    ## # A tibble: 1 × 1
    ##   AverageDeaths
    ##           <dbl>
    ## 1         0.546

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

“Out of 173 listed Avengers, my analysis found that 69 had died at least
one time after they joined the team.” (Adam Zhu)

### Include the code

“The MVP of the Earth-616 Marvel Universe Avengers has to be Jocasta —
an android based on Janet van Dyne and built by Ultron — who has been
destroyed five times and then recovered five times.” - Neel

Answer: Based off the code provided below Jocatsa has diead 5 times and
has returned 5 times - Neel

``` r
jocasta_deaths_detail <- deaths %>%
  filter(Name.Alias == "Jocasta") %>%
  mutate(DeathCount = ifelse(Death == "yes", 1, 0))
jocasta_returns_detail <- returns %>%
  filter(Name.Alias == "Jocasta") %>%
  mutate(ReturnCount = ifelse(Return == "YES", 1, 0))
print("Jocasta's Death Records:")
```

    ## [1] "Jocasta's Death Records:"

``` r
print(jocasta_deaths_detail)
```

    ## # A tibble: 5 × 19
    ##   URL       Name.…¹ Appea…² Curre…³ Gender Proba…⁴ Full.…⁵  Year Years…⁶ Honor…⁷
    ##   <chr>     <chr>     <int> <chr>   <chr>  <chr>   <chr>   <int>   <int> <chr>  
    ## 1 http://m… Jocasta     141 YES     FEMALE Jul-80  Nov-88   1988      27 Full   
    ## 2 http://m… Jocasta     141 YES     FEMALE Jul-80  Nov-88   1988      27 Full   
    ## 3 http://m… Jocasta     141 YES     FEMALE Jul-80  Nov-88   1988      27 Full   
    ## 4 http://m… Jocasta     141 YES     FEMALE Jul-80  Nov-88   1988      27 Full   
    ## 5 http://m… Jocasta     141 YES     FEMALE Jul-80  Nov-88   1988      27 Full   
    ## # … with 9 more variables: Return1 <chr>, Return2 <chr>, Return3 <chr>,
    ## #   Return4 <chr>, Return5 <chr>, Notes <chr>, Time <dbl>, Death <chr>,
    ## #   DeathCount <dbl>, and abbreviated variable names ¹​Name.Alias, ²​Appearances,
    ## #   ³​Current., ⁴​Probationary.Introl, ⁵​Full.Reserve.Avengers.Intro,
    ## #   ⁶​Years.since.joining, ⁷​Honorary

``` r
print("Jocasta's Return Records:")
```

    ## [1] "Jocasta's Return Records:"

``` r
print(jocasta_returns_detail)
```

    ## # A tibble: 5 × 19
    ##   URL       Name.…¹ Appea…² Curre…³ Gender Proba…⁴ Full.…⁵  Year Years…⁶ Honor…⁷
    ##   <chr>     <chr>     <int> <chr>   <chr>  <chr>   <chr>   <int>   <int> <chr>  
    ## 1 http://m… Jocasta     141 YES     FEMALE Jul-80  Nov-88   1988      27 Full   
    ## 2 http://m… Jocasta     141 YES     FEMALE Jul-80  Nov-88   1988      27 Full   
    ## 3 http://m… Jocasta     141 YES     FEMALE Jul-80  Nov-88   1988      27 Full   
    ## 4 http://m… Jocasta     141 YES     FEMALE Jul-80  Nov-88   1988      27 Full   
    ## 5 http://m… Jocasta     141 YES     FEMALE Jul-80  Nov-88   1988      27 Full   
    ## # … with 9 more variables: Death1 <chr>, Death2 <chr>, Death3 <chr>,
    ## #   Death4 <chr>, Death5 <chr>, Notes <chr>, Time <dbl>, Return <chr>,
    ## #   ReturnCount <dbl>, and abbreviated variable names ¹​Name.Alias,
    ## #   ²​Appearances, ³​Current., ⁴​Probationary.Introl,
    ## #   ⁵​Full.Reserve.Avengers.Intro, ⁶​Years.since.joining, ⁷​Honorary

``` r
jocasta_deaths <- jocasta_deaths_detail %>%
  summarise(TotalDeaths = sum(DeathCount, na.rm = TRUE))
jocasta_returns <- jocasta_returns_detail %>%
  summarise(TotalReturns = sum(ReturnCount, na.rm = TRUE))
print("Jocasta's Total Deaths and Returns:")
```

    ## [1] "Jocasta's Total Deaths and Returns:"

``` r
print(jocasta_deaths)
```

    ## # A tibble: 1 × 1
    ##   TotalDeaths
    ##         <dbl>
    ## 1           5

``` r
print(jocasta_returns)
```

    ## # A tibble: 1 × 1
    ##   TotalReturns
    ##          <dbl>
    ## 1            5

``` r
is_jocasta_mvp <- jocasta_deaths$TotalDeaths == 5 & jocasta_returns$TotalReturns == 5
is_jocasta_mvp
```

    ## [1] TRUE

“Of the nine Avengers we see on screen — Iron Man, Hulk, Captain
America, Thor, Hawkeye, Black Widow, Scarlet Witch, Quicksilver and The
Vision — every single one of them has died at least once in the course
of their time Avenging in the comics.” - Ben McGuire

``` r
library(dplyr)


avengers_data <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)


avenger_names <- c("Anthony Edward Stark", "Robert Bruce Banner", "Steven Rogers", "Thor Odinson", "Clinton Francis Barton", 
                   "Natalia Alianovna Romanova", "Wanda Maximoff", "Pietro Maximoff", "Victor Shade (alias)")


result <- sapply(avenger_names, function(name) {
  
  avenger_row <- avengers_data %>% filter(Name.Alias == name)
  

  death_columns <- c("Death1", "Death2", "Death3", "Death4", "Death5")
  

  any(!is.na(avenger_row[death_columns]))
})


result
```

    ##       Anthony Edward Stark        Robert Bruce Banner 
    ##                       TRUE                       TRUE 
    ##              Steven Rogers               Thor Odinson 
    ##                       TRUE                       TRUE 
    ##     Clinton Francis Barton Natalia Alianovna Romanova 
    ##                       TRUE                       TRUE 
    ##             Wanda Maximoff            Pietro Maximoff 
    ##                       TRUE                       TRUE 
    ##       Victor Shade (alias) 
    ##                       TRUE

``` r
all(result)
```

    ## [1] TRUE

Based on the above analyis, it is true that every one of the on-screen
Avengers has died at least once in the Comics. - Ben McGuire

Make sure to include the code to derive the (numeric) fact for the
statement

``` r
# Filter to find Avengers who died at least once
avengers_with_deaths <- deaths %>%
  filter(Death == "yes") %>%
  distinct(Name.Alias) %>%    # Select unique Avengers who died at least once
  count() %>%                 # Count the number of unique Avengers who died
  pull(n)

# Total number of Avengers
total_avengers <- av %>%
  distinct(Name.Alias) %>%
  count() %>%
  pull(n)

# Display results
list(
  Total_Avengers = total_avengers,
  Avengers_With_Deaths = avengers_with_deaths
)
```

    ## $Total_Avengers
    ## [1] 163
    ## 
    ## $Avengers_With_Deaths
    ## [1] 64

``` r
# Adam Zhu
```

### Include your answer

Based on the analysis, the statement provided was incorrect. Here’s the
corrected statement: “Out of 173 listed Avengers, my analysis found that
64 had died at least one time after they joined the team.” This result
indicates that 64 Avengers, not 69, experienced at least one death event
after joining the team. (Adam Zhu)
