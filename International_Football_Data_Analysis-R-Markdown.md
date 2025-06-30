International Footbball Data Analysis R Markdown
================
Shima Mojapelo
2025-04-29

# PHASE 1: *ASK*

As a lifelong fan of football, I have always enjoyed watching
International matches to see the best of each country compete against
each other while showcasing their unique styles of play.

**The Objective:** I’m looking to perform a data analysis to see trends
in winning country’s and teams in international football over the years.

# PHASE 2: *PREPARE*

I found a data set on Kaggle for International Football Results from
1872 to 2025.

## Confirm Data Set is Good

**Relevant:** This data set is very relevant to the ask/objective.

**Original:** The Dataset was made public by its creator, Mart Jürisoo

**Comprehensive:** The dataset seems to be missing a few matches, and
some duplication may be present, however, the size of the dataset
covering ever game for over 50 years may reduce the effect of these
discrepancies.

**Credible:** It has a perfect utility score (10.00) and multiple
positive comments from the community. The Dataset was created by Mart
Jürisoo, a Kaggler trusted community member with several badges for
Dataset Documenting

**Current:** It is updated monthly, so it is up-to-date

## Load the necessary packages and datasets into dataframes

``` r
options(repos = "https://cran.r-project.org")


install.packages("tidyverse")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'tidyverse' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\RtmpkDKu2w\downloaded_packages

``` r
install.packages("dplyr")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'dplyr' successfully unpacked and MD5 sums checked

    ## Warning: cannot remove prior installation of package 'dplyr'

    ## Warning in file.copy(savedcopy, lib, recursive = TRUE): problem copying
    ## C:\Users\shima\AppData\Local\R\win-library\4.5\00LOCK\dplyr\libs\x64\dplyr.dll
    ## to C:\Users\shima\AppData\Local\R\win-library\4.5\dplyr\libs\x64\dplyr.dll:
    ## Permission denied

    ## Warning: restored 'dplyr'

    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\RtmpkDKu2w\downloaded_packages

``` r
install.packages("ggplot2")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'ggplot2' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\RtmpkDKu2w\downloaded_packages

``` r
install.packages("lubridate")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'lubridate' successfully unpacked and MD5 sums checked

    ## Warning: cannot remove prior installation of package 'lubridate'

    ## Warning in file.copy(savedcopy, lib, recursive = TRUE): problem copying
    ## C:\Users\shima\AppData\Local\R\win-library\4.5\00LOCK\lubridate\libs\x64\lubridate.dll
    ## to
    ## C:\Users\shima\AppData\Local\R\win-library\4.5\lubridate\libs\x64\lubridate.dll:
    ## Permission denied

    ## Warning: restored 'lubridate'

    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\RtmpkDKu2w\downloaded_packages

``` r
install.packages("janitor")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'janitor' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\RtmpkDKu2w\downloaded_packages

``` r
install.packages("png")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'png' successfully unpacked and MD5 sums checked

    ## Warning: cannot remove prior installation of package 'png'

    ## Warning in file.copy(savedcopy, lib, recursive = TRUE): problem copying
    ## C:\Users\shima\AppData\Local\R\win-library\4.5\00LOCK\png\libs\x64\png.dll to
    ## C:\Users\shima\AppData\Local\R\win-library\4.5\png\libs\x64\png.dll: Permission
    ## denied

    ## Warning: restored 'png'

    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\RtmpkDKu2w\downloaded_packages

``` r
install.packages("Rtools")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## Warning: package 'Rtools' is not available for this version of R
    ## 
    ## A version of this package for your version of R might be available elsewhere,
    ## see the ideas at
    ## https://cran.r-project.org/doc/manuals/r-patched/R-admin.html#Installing-packages

``` r
#install.packages("sudo") 
install.packages("apt") 
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'apt' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\RtmpkDKu2w\downloaded_packages

``` r
#install.packages("install") 
#install.packages("libcairo2-dev") 
#install.packages("libjpeg9-dev")

library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.2     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.4

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(dplyr)
library(ggplot2)
library(lubridate)
library(janitor)
```

    ## 
    ## Attaching package: 'janitor'
    ## 
    ## The following objects are masked from 'package:stats':
    ## 
    ##     chisq.test, fisher.test

``` r
library(png)

#options(bitmapType='cairo')
#png("xzvf.png")

#library(sudo) 
library(apt) 
```

    ## Loading required package: erer
    ## Loading required package: lmtest
    ## Loading required package: zoo
    ## 
    ## Attaching package: 'zoo'
    ## 
    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric
    ## 
    ## Registered S3 method overwritten by 'quantmod':
    ##   method            from
    ##   as.zoo.data.frame zoo

``` r
#library(install) 
#library(libcairo2-dev) gg
#library(libjpeg9-dev)
```

``` r
original_former_names <- read_csv("Original Data/former_names.csv")
```

    ## Rows: 34 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (2): current, former
    ## date (2): start_date, end_date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
original_goalscorers <- read_csv("Original Data/goalscorers.csv")
```

    ## Rows: 44362 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (4): home_team, away_team, team, scorer
    ## dbl  (1): minute
    ## lgl  (2): own_goal, penalty
    ## date (1): date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
original_results <- read_csv("Original Data/results.csv")
```

    ## Rows: 48207 Columns: 9
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (5): home_team, away_team, tournament, city, country
    ## dbl  (2): home_score, away_score
    ## lgl  (1): neutral
    ## date (1): date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
original_shootouts <- read_csv("Original Data/shootouts.csv")
```

    ## Rows: 645 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (4): home_team, away_team, winner, first_shooter
    ## date (1): date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

## Preview the dataframes

``` r
head(original_former_names)
```

    ## # A tibble: 6 × 4
    ##   current        former                               start_date end_date  
    ##   <chr>          <chr>                                <date>     <date>    
    ## 1 Benin          Dahomey                              1959-11-08 1975-11-30
    ## 2 Burkina Faso   Upper Volta                          1960-04-14 1984-08-04
    ## 3 Curaçao        Netherlands Antilles                 1957-03-03 2010-10-10
    ## 4 Czechoslovakia Bohemia                              1903-04-05 1919-01-01
    ## 5 Czechoslovakia Bohemia and Moravia                  1939-01-01 1945-05-01
    ## 6 Czechoslovakia Representation of Czechs and Slovaks 1993-03-24 1993-11-17

``` r
head(original_goalscorers)
```

    ## # A tibble: 6 × 8
    ##   date       home_team away_team team      scorer        minute own_goal penalty
    ##   <date>     <chr>     <chr>     <chr>     <chr>          <dbl> <lgl>    <lgl>  
    ## 1 1916-07-02 Chile     Uruguay   Uruguay   José Piendib…     44 FALSE    FALSE  
    ## 2 1916-07-02 Chile     Uruguay   Uruguay   Isabelino Gr…     55 FALSE    FALSE  
    ## 3 1916-07-02 Chile     Uruguay   Uruguay   Isabelino Gr…     70 FALSE    FALSE  
    ## 4 1916-07-02 Chile     Uruguay   Uruguay   José Piendib…     75 FALSE    FALSE  
    ## 5 1916-07-06 Argentina Chile     Argentina Alberto Ohaco      2 FALSE    FALSE  
    ## 6 1916-07-06 Argentina Chile     Chile     Telésforo Bá…     44 FALSE    FALSE

``` r
head(original_results)
```

    ## # A tibble: 6 × 9
    ##   date       home_team away_team home_score away_score tournament city   country
    ##   <date>     <chr>     <chr>          <dbl>      <dbl> <chr>      <chr>  <chr>  
    ## 1 1872-11-30 Scotland  England            0          0 Friendly   Glasg… Scotla…
    ## 2 1873-03-08 England   Scotland           4          2 Friendly   London England
    ## 3 1874-03-07 Scotland  England            2          1 Friendly   Glasg… Scotla…
    ## 4 1875-03-06 England   Scotland           2          2 Friendly   London England
    ## 5 1876-03-04 Scotland  England            3          0 Friendly   Glasg… Scotla…
    ## 6 1876-03-25 Scotland  Wales              4          0 Friendly   Glasg… Scotla…
    ## # ℹ 1 more variable: neutral <lgl>

``` r
head(original_shootouts)
```

    ## # A tibble: 6 × 5
    ##   date       home_team   away_team        winner      first_shooter
    ##   <date>     <chr>       <chr>            <chr>       <chr>        
    ## 1 1967-08-22 India       Taiwan           Taiwan      <NA>         
    ## 2 1971-11-14 South Korea Vietnam Republic South Korea <NA>         
    ## 3 1972-05-07 South Korea Iraq             Iraq        <NA>         
    ## 4 1972-05-17 Thailand    South Korea      South Korea <NA>         
    ## 5 1972-05-19 Thailand    Cambodia         Thailand    <NA>         
    ## 6 1973-04-21 Senegal     Ghana            Ghana       <NA>

# PHASE 3: *PROCESS*

## Results Data Processing

### Replace all old country names with new ones For Processed Results

``` r
processed_results <- original_former_names %>%
  rename(country = current)

processed_results <- full_join(processed_results, original_results, by = original_former_names$country, relationship = "many-to-many") 
```

    ## Warning: Unknown or uninitialised column: `country`.

    ## Joining with `by = join_by(country)`

``` r
processed_results$country[processed_results$country %in% original_former_names$former] <- original_former_names$current[original_former_names$former == processed_results$country]
```

    ## Warning in original_former_names$former == processed_results$country: longer
    ## object length is not a multiple of shorter object length

    ## Warning in processed_results$country[processed_results$country %in%
    ## original_former_names$former] <-
    ## original_former_names$current[original_former_names$former == : number of items
    ## to replace is not a multiple of replacement length

``` r
processed_results$home_team[processed_results$home_team %in% original_former_names$former] <- original_former_names$current[original_former_names$former == processed_results$home_team]
```

    ## Warning in original_former_names$former == processed_results$home_team: longer
    ## object length is not a multiple of shorter object length

``` r
processed_results$away_team[processed_results$away_team %in% original_former_names$former] <- original_former_names$current[original_former_names$former == processed_results$away_team]
```

    ## Warning in original_former_names$former == processed_results$away_team: longer
    ## object length is not a multiple of shorter object length

``` r
processed_results <- select(processed_results, date, home_team, away_team, home_score, away_score, tournament, city, country, neutral)

head(processed_results)
```

    ## # A tibble: 6 × 9
    ##   date       home_team away_team  home_score away_score tournament city  country
    ##   <date>     <chr>     <chr>           <dbl>      <dbl> <chr>      <chr> <chr>  
    ## 1 1978-02-12 Benin     Burkina F…          0          2 Friendly   Port… Benin  
    ## 2 1979-04-15 Benin     Ivory Coa…          1          0 African C… Coto… Benin  
    ## 3 1979-06-03 Benin     Togo                0          1 Friendly   Coto… Benin  
    ## 4 1979-07-25 Benin     DR Congo            0          1 Friendly   Port… Benin  
    ## 5 1979-07-28 Benin     DR Congo            0          3 Friendly   Coto… Benin  
    ## 6 1979-10-14 Benin     Nigeria             1          1 Friendly   Coto… Benin  
    ## # ℹ 1 more variable: neutral <lgl>

### Create a column in results to show who won

The new results_with_current_country_names data frame doesn’t have a
column for the winning team. It just has two columns for the home and
away goals.

``` r
processed_results <- mutate(processed_results, home_or_away_win = c(""), winning_team = c(""))

processed_results$home_or_away_win[processed_results$home_score > processed_results$away_score & processed_results$neutral == FALSE] <- "Home"
processed_results$home_or_away_win[processed_results$home_score < processed_results$away_score & processed_results$neutral == FALSE] <- "Away"

processed_results$winning_team[processed_results$home_or_away_win == "Home"] <- processed_results$home_team [processed_results$home_or_away_win == "Home"]
processed_results$winning_team[processed_results$home_or_away_win == "Away"] <- processed_results$away_team[processed_results$home_or_away_win == "Away"]

head(processed_results)
```

    ## # A tibble: 6 × 11
    ##   date       home_team away_team  home_score away_score tournament city  country
    ##   <date>     <chr>     <chr>           <dbl>      <dbl> <chr>      <chr> <chr>  
    ## 1 1978-02-12 Benin     Burkina F…          0          2 Friendly   Port… Benin  
    ## 2 1979-04-15 Benin     Ivory Coa…          1          0 African C… Coto… Benin  
    ## 3 1979-06-03 Benin     Togo                0          1 Friendly   Coto… Benin  
    ## 4 1979-07-25 Benin     DR Congo            0          1 Friendly   Port… Benin  
    ## 5 1979-07-28 Benin     DR Congo            0          3 Friendly   Coto… Benin  
    ## 6 1979-10-14 Benin     Nigeria             1          1 Friendly   Coto… Benin  
    ## # ℹ 3 more variables: neutral <lgl>, home_or_away_win <chr>, winning_team <chr>

### Create Composite Key

``` r
processed_results <- mutate(processed_results, matchID = paste(date, home_team, away_team))
```

## Original Shootouts Data Processing

### Replace all old country names with new ones For Processed Shootouts

``` r
processed_shootouts <- original_shootouts

processed_shootouts$home_team[processed_shootouts$home_team %in% original_former_names$former] <- original_former_names$current[original_former_names$former == processed_shootouts$home_team]
```

    ## Warning in original_former_names$former == processed_shootouts$home_team:
    ## longer object length is not a multiple of shorter object length

``` r
processed_shootouts$away_team[processed_shootouts$away_team %in% original_former_names$former] <- original_former_names$current[original_former_names$former == processed_shootouts$away_team]
```

    ## Warning in original_former_names$former == processed_shootouts$away_team:
    ## longer object length is not a multiple of shorter object length

``` r
processed_shootouts$first_shooter[processed_shootouts$first_shooter %in% original_former_names$former] <- original_former_names$current[original_former_names$former == processed_shootouts$first_shooter]
```

    ## Warning in original_former_names$former == processed_shootouts$first_shooter:
    ## longer object length is not a multiple of shorter object length

``` r
head(processed_shootouts)
```

    ## # A tibble: 6 × 5
    ##   date       home_team   away_team        winner      first_shooter
    ##   <date>     <chr>       <chr>            <chr>       <chr>        
    ## 1 1967-08-22 India       Taiwan           Taiwan      <NA>         
    ## 2 1971-11-14 South Korea Vietnam Republic South Korea <NA>         
    ## 3 1972-05-07 South Korea Iraq             Iraq        <NA>         
    ## 4 1972-05-17 Thailand    South Korea      South Korea <NA>         
    ## 5 1972-05-19 Thailand    Cambodia         Thailand    <NA>         
    ## 6 1973-04-21 Senegal     Ghana            Ghana       <NA>

### Add a column in shootouts data to show if it was a home or away win

``` r
processed_shootouts <- mutate(original_shootouts, home_or_away_win = c(""))

processed_shootouts$home_or_away_win[processed_shootouts$winner == processed_shootouts$home_team] <- "Home"
processed_shootouts$home_or_away_win[processed_shootouts$winner == processed_shootouts$away_team] <- "Away"

head(processed_shootouts)
```

    ## # A tibble: 6 × 6
    ##   date       home_team   away_team        winner  first_shooter home_or_away_win
    ##   <date>     <chr>       <chr>            <chr>   <chr>         <chr>           
    ## 1 1967-08-22 India       Taiwan           Taiwan  <NA>          Away            
    ## 2 1971-11-14 South Korea Vietnam Republic South … <NA>          Home            
    ## 3 1972-05-07 South Korea Iraq             Iraq    <NA>          Away            
    ## 4 1972-05-17 Thailand    South Korea      South … <NA>          Away            
    ## 5 1972-05-19 Thailand    Cambodia         Thaila… <NA>          Home            
    ## 6 1973-04-21 Senegal     Ghana            Ghana   <NA>          Away

### Rename Winner column

``` r
processed_shootouts <- rename(processed_shootouts, shootout_winner = winner)
```

### Create Composite Key For Processed Shootouts

``` r
processed_shootouts <- mutate(processed_shootouts, matchID = paste(date, home_team, away_team))
```

## Original Goalscorers Data Processing

### Replace all old country names with new ones For Processed Goalscorers

``` r
processed_goalscorers <- original_goalscorers

processed_goalscorers$home_team[processed_goalscorers$home_team %in% original_former_names$former] <- original_former_names$current[original_former_names$former == processed_goalscorers$home_team]
```

    ## Warning in original_former_names$former == processed_goalscorers$home_team:
    ## longer object length is not a multiple of shorter object length

``` r
processed_goalscorers$away_team[processed_goalscorers$away_team %in% original_former_names$former] <- original_former_names$current[original_former_names$former == processed_goalscorers$away_team]
```

    ## Warning in original_former_names$former == processed_goalscorers$away_team:
    ## longer object length is not a multiple of shorter object length

``` r
processed_goalscorers$team[processed_goalscorers$team %in% original_former_names$former] <- original_former_names$current[original_former_names$former == processed_goalscorers$team]
```

    ## Warning in original_former_names$former == processed_goalscorers$team: longer
    ## object length is not a multiple of shorter object length

``` r
head(processed_goalscorers)
```

    ## # A tibble: 6 × 8
    ##   date       home_team away_team team      scorer        minute own_goal penalty
    ##   <date>     <chr>     <chr>     <chr>     <chr>          <dbl> <lgl>    <lgl>  
    ## 1 1916-07-02 Chile     Uruguay   Uruguay   José Piendib…     44 FALSE    FALSE  
    ## 2 1916-07-02 Chile     Uruguay   Uruguay   Isabelino Gr…     55 FALSE    FALSE  
    ## 3 1916-07-02 Chile     Uruguay   Uruguay   Isabelino Gr…     70 FALSE    FALSE  
    ## 4 1916-07-02 Chile     Uruguay   Uruguay   José Piendib…     75 FALSE    FALSE  
    ## 5 1916-07-06 Argentina Chile     Argentina Alberto Ohaco      2 FALSE    FALSE  
    ## 6 1916-07-06 Argentina Chile     Chile     Telésforo Bá…     44 FALSE    FALSE

### Add column to show if goal was scored in original time or extra time

``` r
processed_goalscorers <- mutate(processed_goalscorers, extra_time = c(""))

processed_goalscorers$extra_time[processed_goalscorers$minute <= 90 & processed_goalscorers$minute > 0] <- FALSE
processed_goalscorers$extra_time[processed_goalscorers$minute > 90] <- TRUE

head(processed_goalscorers)
```

    ## # A tibble: 6 × 9
    ##   date       home_team away_team team  scorer minute own_goal penalty extra_time
    ##   <date>     <chr>     <chr>     <chr> <chr>   <dbl> <lgl>    <lgl>   <chr>     
    ## 1 1916-07-02 Chile     Uruguay   Urug… José …     44 FALSE    FALSE   FALSE     
    ## 2 1916-07-02 Chile     Uruguay   Urug… Isabe…     55 FALSE    FALSE   FALSE     
    ## 3 1916-07-02 Chile     Uruguay   Urug… Isabe…     70 FALSE    FALSE   FALSE     
    ## 4 1916-07-02 Chile     Uruguay   Urug… José …     75 FALSE    FALSE   FALSE     
    ## 5 1916-07-06 Argentina Chile     Arge… Alber…      2 FALSE    FALSE   FALSE     
    ## 6 1916-07-06 Argentina Chile     Chile Telés…     44 FALSE    FALSE   FALSE

### Add column to track goal count for each goalscorer

``` r
processed_goalscorers <- mutate(processed_goalscorers, goalscorer_tally = c(0)) 

processed_goalscorers$goalscorer_tally[processed_goalscorers$scorer %in% processed_goalscorers$scorer] <- ave(processed_goalscorers$scorer == processed_goalscorers$scorer, processed_goalscorers$scorer, FUN = cumsum)

head(processed_goalscorers)
```

    ## # A tibble: 6 × 10
    ##   date       home_team away_team team  scorer minute own_goal penalty extra_time
    ##   <date>     <chr>     <chr>     <chr> <chr>   <dbl> <lgl>    <lgl>   <chr>     
    ## 1 1916-07-02 Chile     Uruguay   Urug… José …     44 FALSE    FALSE   FALSE     
    ## 2 1916-07-02 Chile     Uruguay   Urug… Isabe…     55 FALSE    FALSE   FALSE     
    ## 3 1916-07-02 Chile     Uruguay   Urug… Isabe…     70 FALSE    FALSE   FALSE     
    ## 4 1916-07-02 Chile     Uruguay   Urug… José …     75 FALSE    FALSE   FALSE     
    ## 5 1916-07-06 Argentina Chile     Arge… Alber…      2 FALSE    FALSE   FALSE     
    ## 6 1916-07-06 Argentina Chile     Chile Telés…     44 FALSE    FALSE   FALSE     
    ## # ℹ 1 more variable: goalscorer_tally <dbl>

### Add column to track goal count for each game

``` r
processed_goalscorers <- mutate(processed_goalscorers, goals_in_match = c(0)) 

processed_goalscorers$goals_in_match[processed_goalscorers$date %in% processed_goalscorers$date & processed_goalscorers$home_team %in% processed_goalscorers$home_team & processed_goalscorers$away_team %in% processed_goalscorers$away_team] <- ave(processed_goalscorers$date == processed_goalscorers$date & processed_goalscorers$home_team == processed_goalscorers$home_team & processed_goalscorers$away_team == processed_goalscorers$away_team, processed_goalscorers$date,processed_goalscorers$home_team,processed_goalscorers$away_team, FUN = cumsum)

head(processed_goalscorers)
```

    ## # A tibble: 6 × 11
    ##   date       home_team away_team team  scorer minute own_goal penalty extra_time
    ##   <date>     <chr>     <chr>     <chr> <chr>   <dbl> <lgl>    <lgl>   <chr>     
    ## 1 1916-07-02 Chile     Uruguay   Urug… José …     44 FALSE    FALSE   FALSE     
    ## 2 1916-07-02 Chile     Uruguay   Urug… Isabe…     55 FALSE    FALSE   FALSE     
    ## 3 1916-07-02 Chile     Uruguay   Urug… Isabe…     70 FALSE    FALSE   FALSE     
    ## 4 1916-07-02 Chile     Uruguay   Urug… José …     75 FALSE    FALSE   FALSE     
    ## 5 1916-07-06 Argentina Chile     Arge… Alber…      2 FALSE    FALSE   FALSE     
    ## 6 1916-07-06 Argentina Chile     Chile Telés…     44 FALSE    FALSE   FALSE     
    ## # ℹ 2 more variables: goalscorer_tally <dbl>, goals_in_match <dbl>

### Rename team column

``` r
processed_goalscorers <- rename(processed_goalscorers, scorers_team = team)
```

### Create Composite Key For Processed Goalscorers

``` r
processed_goalscorers <- mutate(processed_goalscorers, matchID = paste(date, home_team,away_team))
```

### Sort Data

``` r
processed_goalscorers <- arrange(processed_goalscorers, processed_goalscorers$date, processed_goalscorers$home_team, processed_goalscorers$away_team, processed_goalscorers$minute)
```

### Add column to track if goals were scored in the first half or second half

This is for those that were not scored in extra time)

``` r
processed_goalscorers <- mutate(processed_goalscorers, first_or_second_half_or_extra_time_goal= c(""))

processed_goalscorers$first_or_second_half_or_extra_time_goal[processed_goalscorers$minute <= 45] <- "first"

processed_goalscorers$first_or_second_half_or_extra_time_goal[processed_goalscorers$minute > 45 & processed_goalscorers$minute <= 90] <- "second"

processed_goalscorers$first_or_second_half_or_extra_time_goal[processed_goalscorers$minute > 90] <- "extra"
```

## write CSV for visualisations in Tableau

``` r
write.csv(processed_goalscorers, "Cleaned Data/Processed Goalscorer Data.csv")
write.csv(processed_results, "Cleaned Data/Processed Results Data.csv")
write.csv(processed_shootouts, "Cleaned Data/Processed Shootouts Data.csv")
```

# PHASE 4: *ANALYSE*

## R Analysis Calculations

### Calculate how often a team scoring first leads to a win

``` r
results_with_goals <- filter(processed_results, winning_team != "" & (home_score > 0 | away_score > 0)) %>%
  filter(date >= "1916-07-02" & date <= "2024-07-14") #goalscorer data was only captured from 1916-07-02 to 2024-07-14
results_with_goals
```

    ## # A tibble: 27,339 × 12
    ##    date       home_team away_team home_score away_score tournament city  country
    ##    <date>     <chr>     <chr>          <dbl>      <dbl> <chr>      <chr> <chr>  
    ##  1 1978-02-12 Benin     Burkina …          0          2 Friendly   Port… Benin  
    ##  2 1979-04-15 Benin     Ivory Co…          1          0 African C… Coto… Benin  
    ##  3 1979-06-03 Benin     Togo               0          1 Friendly   Coto… Benin  
    ##  4 1979-07-25 Benin     DR Congo           0          1 Friendly   Port… Benin  
    ##  5 1979-07-28 Benin     DR Congo           0          3 Friendly   Coto… Benin  
    ##  6 1980-11-15 Benin     Niger              1          0 Friendly   Coto… Benin  
    ##  7 1981-07-16 Benin     DR Congo           1          2 Friendly   Coto… Benin  
    ##  8 1981-07-19 Benin     Ghana              1          2 Friendly   Coto… Benin  
    ##  9 1982-01-24 Benin     Mali               2          3 Friendly   Coto… Benin  
    ## 10 1982-02-13 Benin     Ghana              0          4 West Afri… Coto… Benin  
    ## # ℹ 27,329 more rows
    ## # ℹ 4 more variables: neutral <lgl>, home_or_away_win <chr>,
    ## #   winning_team <chr>, matchID <chr>

``` r
total_number_of_matches_with_goals <- count(results_with_goals)
total_number_of_matches_with_goals
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1 27339

``` r
first_scorers <- filter(processed_goalscorers, goals_in_match == 1 & own_goal == FALSE)
first_scorers
```

    ## # A tibble: 14,073 × 13
    ##    date       home_team away_team scorers_team scorer    minute own_goal penalty
    ##    <date>     <chr>     <chr>     <chr>        <chr>      <dbl> <lgl>    <lgl>  
    ##  1 1916-07-02 Chile     Uruguay   Uruguay      José Pie…     44 FALSE    FALSE  
    ##  2 1916-07-06 Argentina Chile     Argentina    Alberto …      2 FALSE    FALSE  
    ##  3 1916-07-08 Brazil    Chile     Brazil       Demósten…     29 FALSE    FALSE  
    ##  4 1916-07-10 Argentina Brazil    Argentina    José Dur…     10 FALSE    FALSE  
    ##  5 1916-07-12 Brazil    Uruguay   Brazil       Arthur F…      8 FALSE    FALSE  
    ##  6 1917-09-30 Uruguay   Chile     Uruguay      Carlos S…     20 FALSE    FALSE  
    ##  7 1917-10-03 Argentina Brazil    Brazil       Neco           8 FALSE    FALSE  
    ##  8 1917-10-07 Uruguay   Brazil    Uruguay      Héctor S…      8 FALSE    FALSE  
    ##  9 1917-10-12 Brazil    Chile     Brazil       Caetano …     21 FALSE    FALSE  
    ## 10 1917-10-14 Uruguay   Argentina Uruguay      Héctor S…     62 FALSE    FALSE  
    ## # ℹ 14,063 more rows
    ## # ℹ 5 more variables: extra_time <chr>, goalscorer_tally <dbl>,
    ## #   goals_in_match <dbl>, matchID <chr>,
    ## #   first_or_second_half_or_extra_time_goal <chr>

``` r
results_with_goals_and_record_of_goalscorers <- filter(results_with_goals, results_with_goals$matchID %in% first_scorers$matchID)
results_with_goals_and_record_of_goalscorers
```

    ## # A tibble: 9,227 × 12
    ##    date       home_team away_team home_score away_score tournament city  country
    ##    <date>     <chr>     <chr>          <dbl>      <dbl> <chr>      <chr> <chr>  
    ##  1 1984-10-28 Benin     Tunisia            0          2 FIFA Worl… Coto… Benin  
    ##  2 1992-10-25 Benin     Morocco            0          1 FIFA Worl… Coto… Benin  
    ##  3 1993-01-17 Benin     Tunisia            0          5 FIFA Worl… Coto… Benin  
    ##  4 1993-02-28 Benin     Ethiopia           1          0 FIFA Worl… Coto… Benin  
    ##  5 2003-11-16 Benin     Madagasc…          3          2 FIFA Worl… Coto… Benin  
    ##  6 2004-10-10 Benin     Ivory Co…          0          1 FIFA Worl… Coto… Benin  
    ##  7 2005-06-04 Benin     Cameroon           1          4 FIFA Worl… Coto… Benin  
    ##  8 2005-10-09 Benin     Libya              1          0 FIFA Worl… Coto… Benin  
    ##  9 2008-06-08 Benin     Uganda             4          1 FIFA Worl… Coto… Benin  
    ## 10 2008-06-22 Benin     Niger              2          0 FIFA Worl… Coto… Benin  
    ## # ℹ 9,217 more rows
    ## # ℹ 4 more variables: neutral <lgl>, home_or_away_win <chr>,
    ## #   winning_team <chr>, matchID <chr>

``` r
scoring_first_and_result <- full_join(results_with_goals_and_record_of_goalscorers,first_scorers,  by = "matchID", relationship = "many-to-many") %>%
  select(date.x, home_team.x, away_team.x, scorers_team, winning_team) 
scoring_first_and_result
```

    ## # A tibble: 14,370 × 5
    ##    date.x     home_team.x away_team.x scorers_team winning_team
    ##    <date>     <chr>       <chr>       <chr>        <chr>       
    ##  1 1984-10-28 Benin       Tunisia     Tunisia      Tunisia     
    ##  2 1992-10-25 Benin       Morocco     Morocco      Morocco     
    ##  3 1993-01-17 Benin       Tunisia     Tunisia      Tunisia     
    ##  4 1993-02-28 Benin       Ethiopia    Benin        Benin       
    ##  5 2003-11-16 Benin       Madagascar  Madagascar   Benin       
    ##  6 2004-10-10 Benin       Ivory Coast Ivory Coast  Ivory Coast 
    ##  7 2005-06-04 Benin       Cameroon    Cameroon     Cameroon    
    ##  8 2005-10-09 Benin       Libya       Benin        Benin       
    ##  9 2008-06-08 Benin       Uganda      Uganda       Benin       
    ## 10 2008-06-22 Benin       Niger       Benin        Benin       
    ## # ℹ 14,360 more rows

``` r
scoring_first_and_winning <- filter(scoring_first_and_result, scorers_team == winning_team) %>%
  count()
  
scoring_first_and_winning
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1  8295

``` r
percentage_of_teams_scoring_first_and_winning <- (scoring_first_and_winning / count(results_with_goals_and_record_of_goalscorers)) * 100
percentage_of_teams_scoring_first_and_winning
```

    ##          n
    ## 1 89.89921

``` r
percentage_of_teams_scoring_first_and_losing <- ((count(results_with_goals_and_record_of_goalscorers) - scoring_first_and_winning)/ count(results_with_goals_and_record_of_goalscorers)) * 100
percentage_of_teams_scoring_first_and_losing
```

    ##          n
    ## 1 10.10079

``` r
# create Pie Chart to visualise data
slices <- c(as.numeric(percentage_of_teams_scoring_first_and_winning), as.numeric(percentage_of_teams_scoring_first_and_losing))
slices <- round(slices/sum(slices)*100)

pie_labels <- c("Scoring First and Winning", "Scoring First And Losing") %>%
  paste(round(slices/sum(slices)*100, 2)) %>%
  paste("%",sep="")

#setwd("C:/Users/shima/OneDrive/NEVER DELETE/Data Analytics/Google Data Analytics Certificate/Capstone Projects/International_Football_Data_Analysis")

#png(filename="/International_Football_Data_Analysis-R-Markdown_files/figure-gf/scoring first and winning.png", width=800, height=800)

while (!is.null(dev.list()))  dev.off()

pie(slices, labels = pie_labels, main = "Scoring First And Winning")
```

``` r
scoring_first_by_country_correlation_x <- filter(processed_goalscorers, goals_in_match == 1 & own_goal == FALSE) %>%
  group_by(scorers_team) %>%
  count()
scoring_first_by_country_correlation_x
```

    ## # A tibble: 214 × 2
    ## # Groups:   scorers_team [214]
    ##    scorers_team            n
    ##    <chr>               <int>
    ##  1 Afghanistan             7
    ##  2 Albania                72
    ##  3 Algeria                88
    ##  4 American Samoa          3
    ##  5 Andorra                10
    ##  6 Angola                 41
    ##  7 Antigua and Barbuda    19
    ##  8 Argentina             283
    ##  9 Armenia                57
    ## 10 Aruba                   2
    ## # ℹ 204 more rows

``` r
total_wins_by_country_correlation_y <- processed_results %>%
  group_by(processed_results$winning_team) %>%
  count()
total_wins_by_country_correlation_y
```

    ## # A tibble: 286 × 2
    ## # Groups:   processed_results$winning_team [286]
    ##    `processed_results$winning_team`     n
    ##    <chr>                            <int>
    ##  1 ""                               21153
    ##  2 "Abkhazia"                           7
    ##  3 "Afghanistan"                        8
    ##  4 "Albania"                           94
    ##  5 "Alderney"                           1
    ##  6 "Algeria"                          205
    ##  7 "American Samoa"                     1
    ##  8 "Andalusia"                          8
    ##  9 "Andorra"                           12
    ## 10 "Angola"                           110
    ## # ℹ 276 more rows

``` r
total_scoring_first_vs_total_wins_by_country <- inner_join(scoring_first_by_country_correlation_x, total_wins_by_country_correlation_y, by = c("scorers_team" = "processed_results$winning_team"))
total_scoring_first_vs_total_wins_by_country
```

    ## # A tibble: 214 × 3
    ## # Groups:   scorers_team [214]
    ##    scorers_team          n.x   n.y
    ##    <chr>               <int> <int>
    ##  1 Afghanistan             7     8
    ##  2 Albania                72    94
    ##  3 Algeria                88   205
    ##  4 American Samoa          3     1
    ##  5 Andorra                10    12
    ##  6 Angola                 41   110
    ##  7 Antigua and Barbuda    19    53
    ##  8 Argentina             283   396
    ##  9 Armenia                57    54
    ## 10 Aruba                   2    24
    ## # ℹ 204 more rows

``` r
total_scoring_first_vs_total_wins_by_country_correlation_coeeficient <- cor(x = total_scoring_first_vs_total_wins_by_country$n.x, y = total_scoring_first_vs_total_wins_by_country$n.y)
total_scoring_first_vs_total_wins_by_country_correlation_coeeficient
```

    ## [1] 0.89083

``` r
while (!is.null(dev.list()))  dev.off()

ggplot(DATA = total_scoring_first_vs_total_wins_by_country) + geom_point(aes(x = total_scoring_first_vs_total_wins_by_country$n.x, y = total_scoring_first_vs_total_wins_by_country$n.y)) + geom_smooth(aes(x = total_scoring_first_vs_total_wins_by_country$n.x, y = total_scoring_first_vs_total_wins_by_country$n.y), method = "lm", size = 1, se = FALSE, linetype = 1,color = "red") + labs(x = "total scoring first", y = "total wins", title = "total scoring first vs total wins by country") 
```

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

    ## `geom_smooth()` using formula = 'y ~ x'

### What is the correlation between being winning the most and scoring the most goals

``` r
total_wins_by_country_correlation_y <- processed_results %>%
  group_by(processed_results$winning_team) %>%
  count()
total_wins_by_country_correlation_y
```

    ## # A tibble: 286 × 2
    ## # Groups:   processed_results$winning_team [286]
    ##    `processed_results$winning_team`     n
    ##    <chr>                            <int>
    ##  1 ""                               21153
    ##  2 "Abkhazia"                           7
    ##  3 "Afghanistan"                        8
    ##  4 "Albania"                           94
    ##  5 "Alderney"                           1
    ##  6 "Algeria"                          205
    ##  7 "American Samoa"                     1
    ##  8 "Andalusia"                          8
    ##  9 "Andorra"                           12
    ## 10 "Angola"                           110
    ## # ℹ 276 more rows

``` r
total_goals_by_country_correlation_x <- processed_goalscorers %>%
  group_by(processed_goalscorers$scorers_team) %>%
  count()

total_goals_by_country_correlation_x
```

    ## # A tibble: 220 × 2
    ## # Groups:   processed_goalscorers$scorers_team [220]
    ##    `processed_goalscorers$scorers_team`     n
    ##    <chr>                                <int>
    ##  1 Afghanistan                             15
    ##  2 Albania                                198
    ##  3 Algeria                                260
    ##  4 American Samoa                          10
    ##  5 Andorra                                 48
    ##  6 Angola                                 129
    ##  7 Anguilla                                 2
    ##  8 Antigua and Barbuda                     64
    ##  9 Argentina                              948
    ## 10 Armenia                                162
    ## # ℹ 210 more rows

``` r
total_wins_vs_total_goals_by_country <- inner_join(total_goals_by_country_correlation_x, total_wins_by_country_correlation_y, by = c("processed_goalscorers$scorers_team" = "processed_results$winning_team"))
total_wins_vs_total_goals_by_country
```

    ## # A tibble: 220 × 3
    ## # Groups:   processed_goalscorers$scorers_team [220]
    ##    `processed_goalscorers$scorers_team`   n.x   n.y
    ##    <chr>                                <int> <int>
    ##  1 Afghanistan                             15     8
    ##  2 Albania                                198    94
    ##  3 Algeria                                260   205
    ##  4 American Samoa                          10     1
    ##  5 Andorra                                 48    12
    ##  6 Angola                                 129   110
    ##  7 Anguilla                                 2     3
    ##  8 Antigua and Barbuda                     64    53
    ##  9 Argentina                              948   396
    ## 10 Armenia                                162    54
    ## # ℹ 210 more rows

``` r
total_wins_vs_total_goals_by_country_correlation_coeeficient <- cor(x = total_wins_vs_total_goals_by_country$n.x, y = total_wins_vs_total_goals_by_country$n.y)
total_wins_vs_total_goals_by_country_correlation_coeeficient
```

    ## [1] 0.8871294

``` r
while (!is.null(dev.list()))  dev.off()

ggplot(DATA = total_wins_vs_total_goals_by_country) + geom_point(aes(x = total_wins_vs_total_goals_by_country$n.x, y = total_wins_vs_total_goals_by_country$n.y)) + geom_smooth(aes(x = total_wins_vs_total_goals_by_country$n.x, y = total_wins_vs_total_goals_by_country$n.y), method = "lm", size = 1, se = FALSE, linetype = 1,
              color = "red") + labs(x = "total goals", y = "total wins", title = "total goals vs total wins by country") 
```

    ## `geom_smooth()` using formula = 'y ~ x'

## Tableau Data Viz

Prepare Visualisations in Tableau

<div id="viz1746524647108" class="tableauPlaceholder"
style="position: relative">

<noscript>
<a href='#'><img alt='What Makes A Winning International Football Team Over Time? ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Da&#47;DataVisualisation_17464310848170&#47;Story1&#47;1_rss.png' style='border: none' /></a>
</noscript>
<object class="tableauViz" style="display:none;">
<param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' />
<param name='embed_code_version' value='3' />
<param name='path' value='shared&#47;' />
<param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;&#47;&#47;1.png' />
<param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' />
</object>

</div>

``` js

var divElement = document.getElementById('viz1746524647108');                    
var vizElement = divElement.getElementsByTagName('object')[0];                    
vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    
var scriptElement = document.createElement('script');                    
scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    
vizElement.parentNode.insertBefore(scriptElement, vizElement);                        
```

<script>
&#10;var divElement = document.getElementById('viz1746524647108');                    
var vizElement = divElement.getElementsByTagName('object')[0];                    
vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    
var scriptElement = document.createElement('script');                    
scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    
vizElement.parentNode.insertBefore(scriptElement, vizElement);                        
&#10;</script>
# PHASE 5: *SHARE*

Present clear findings and insights from each of the data visualisations

**Finding:** Countries with a lot of goals DON’T necessarily win a lot
**Insight:** *Scoring a lot of goals isn’t the most important part of
football*

**Finding:** Home teams win a lot more than away teams **Insight:** *Try
to have the highest chance of success*

**Finding:** There’s a direct correlation between a teams ability to
score first and its ability to win games **Insight:** *Succesful teams
focus on scoring first and defending* **Recommendation:** do more
research and find data on defense and midfield statistics (this data
mainly speaks to offense)
