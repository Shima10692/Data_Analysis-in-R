Bellabeat Analysis R Markdown
================
Shima Mojapelo
2025-04-25

# **PHASE 1: ASK**

Bellabeat is a high-tech manufacturer of health-focused products for
women. As a data analyst in the marketing analyst team, I have been
requested by Urška Sršen, cofounder and Chief Creative Officer of
Bellabeat, to analyse smart device fitness data to identify potential
growth opportunities for the company. THe objective is to gain insights
into how consumers use their smart devices in order to help inform the
marketing strategy.

Our main questions are: 1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers? 3. How could
these trends help influence Bellabeat marketing strategy?

# **PHASE 2: PREPARE**

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
    ##  C:\Users\shima\AppData\Local\Temp\Rtmp6TRt4b\downloaded_packages

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
    ##  C:\Users\shima\AppData\Local\Temp\Rtmp6TRt4b\downloaded_packages

``` r
install.packages("ggplot2")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'ggplot2' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\Rtmp6TRt4b\downloaded_packages

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
    ##  C:\Users\shima\AppData\Local\Temp\Rtmp6TRt4b\downloaded_packages

``` r
install.packages("sqldf")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'sqldf' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\Rtmp6TRt4b\downloaded_packages

``` r
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
library(sqldf)
```

    ## Loading required package: gsubfn
    ## Loading required package: proto
    ## Loading required package: RSQLite

## Load datasets from 3.12.16-4.11.16 into dataframes

``` r
daily_Activity_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/dailyActivity_merged.csv") 
```

    ## Rows: 457 Columns: 15
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityDate
    ## dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDi...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(daily_Activity_merged1)
```

    ## # A tibble: 6 × 15
    ##           Id ActivityDate TotalSteps TotalDistance TrackerDistance
    ##        <dbl> <chr>             <dbl>         <dbl>           <dbl>
    ## 1 1503960366 3/25/2016         11004          7.11            7.11
    ## 2 1503960366 3/26/2016         17609         11.6            11.6 
    ## 3 1503960366 3/27/2016         12736          8.53            8.53
    ## 4 1503960366 3/28/2016         13231          8.93            8.93
    ## 5 1503960366 3/29/2016         12041          7.85            7.85
    ## 6 1503960366 3/30/2016         10970          7.16            7.16
    ## # ℹ 10 more variables: LoggedActivitiesDistance <dbl>,
    ## #   VeryActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
    ## #   LightActiveDistance <dbl>, SedentaryActiveDistance <dbl>,
    ## #   VeryActiveMinutes <dbl>, FairlyActiveMinutes <dbl>,
    ## #   LightlyActiveMinutes <dbl>, SedentaryMinutes <dbl>, Calories <dbl>

``` r
heartrate_seconds_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/heartrate_seconds_merged.csv")
```

    ## Rows: 1154681 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Time
    ## dbl (2): Id, Value
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(heartrate_seconds_merged1)
```

    ## # A tibble: 6 × 3
    ##           Id Time                Value
    ##        <dbl> <chr>               <dbl>
    ## 1 2022484408 4/1/2016 7:54:00 AM    93
    ## 2 2022484408 4/1/2016 7:54:05 AM    91
    ## 3 2022484408 4/1/2016 7:54:10 AM    96
    ## 4 2022484408 4/1/2016 7:54:15 AM    98
    ## 5 2022484408 4/1/2016 7:54:20 AM   100
    ## 6 2022484408 4/1/2016 7:54:25 AM   101

``` r
hourlyCalories_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/hourlyCalories_merged.csv")
```

    ## Rows: 24084 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (2): Id, Calories
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(hourlyCalories_merged1)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityHour          Calories
    ##        <dbl> <chr>                    <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM       48
    ## 2 1503960366 3/12/2016 1:00:00 AM        48
    ## 3 1503960366 3/12/2016 2:00:00 AM        48
    ## 4 1503960366 3/12/2016 3:00:00 AM        48
    ## 5 1503960366 3/12/2016 4:00:00 AM        48
    ## 6 1503960366 3/12/2016 5:00:00 AM        48

``` r
hourlyIntensities_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/hourlyIntensities_merged.csv")
```

    ## Rows: 24084 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (3): Id, TotalIntensity, AverageIntensity
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(hourlyIntensities_merged1)
```

    ## # A tibble: 6 × 4
    ##           Id ActivityHour          TotalIntensity AverageIntensity
    ##        <dbl> <chr>                          <dbl>            <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM              0                0
    ## 2 1503960366 3/12/2016 1:00:00 AM               0                0
    ## 3 1503960366 3/12/2016 2:00:00 AM               0                0
    ## 4 1503960366 3/12/2016 3:00:00 AM               0                0
    ## 5 1503960366 3/12/2016 4:00:00 AM               0                0
    ## 6 1503960366 3/12/2016 5:00:00 AM               0                0

``` r
hourlySteps_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/hourlySteps_merged.csv")
```

    ## Rows: 24084 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (2): Id, StepTotal
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(hourlySteps_merged1)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityHour          StepTotal
    ##        <dbl> <chr>                     <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM         0
    ## 2 1503960366 3/12/2016 1:00:00 AM          0
    ## 3 1503960366 3/12/2016 2:00:00 AM          0
    ## 4 1503960366 3/12/2016 3:00:00 AM          0
    ## 5 1503960366 3/12/2016 4:00:00 AM          0
    ## 6 1503960366 3/12/2016 5:00:00 AM          0

``` r
minuteCaloriesNarrow_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/minuteCaloriesNarrow_merged.csv")
```

    ## Rows: 1445040 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityMinute
    ## dbl (2): Id, Calories
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteCaloriesNarrow_merged1)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute        Calories
    ##        <dbl> <chr>                    <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM    0.797
    ## 2 1503960366 3/12/2016 12:01:00 AM    0.797
    ## 3 1503960366 3/12/2016 12:02:00 AM    0.797
    ## 4 1503960366 3/12/2016 12:03:00 AM    0.797
    ## 5 1503960366 3/12/2016 12:04:00 AM    0.797
    ## 6 1503960366 3/12/2016 12:05:00 AM    0.797

``` r
minuteIntensitiesNarrow_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/minuteIntensitiesNarrow_merged.csv")
```

    ## Rows: 1445040 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityMinute
    ## dbl (2): Id, Intensity
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteIntensitiesNarrow_merged1)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute        Intensity
    ##        <dbl> <chr>                     <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM         0
    ## 2 1503960366 3/12/2016 12:01:00 AM         0
    ## 3 1503960366 3/12/2016 12:02:00 AM         0
    ## 4 1503960366 3/12/2016 12:03:00 AM         0
    ## 5 1503960366 3/12/2016 12:04:00 AM         0
    ## 6 1503960366 3/12/2016 12:05:00 AM         0

``` r
minuteMETsNarrow_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/minuteMETsNarrow_merged.csv")
```

    ## Rows: 1445040 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityMinute
    ## dbl (2): Id, METs
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteMETsNarrow_merged1)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute         METs
    ##        <dbl> <chr>                 <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM    10
    ## 2 1503960366 3/12/2016 12:01:00 AM    10
    ## 3 1503960366 3/12/2016 12:02:00 AM    10
    ## 4 1503960366 3/12/2016 12:03:00 AM    10
    ## 5 1503960366 3/12/2016 12:04:00 AM    10
    ## 6 1503960366 3/12/2016 12:05:00 AM    10

``` r
minuteSleep_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/minuteSleep_merged.csv")
```

    ## Rows: 198559 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): date
    ## dbl (3): Id, value, logId
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteSleep_merged1)
```

    ## # A tibble: 6 × 4
    ##           Id date                 value       logId
    ##        <dbl> <chr>                <dbl>       <dbl>
    ## 1 1503960366 3/13/2016 2:39:30 AM     1 11114919637
    ## 2 1503960366 3/13/2016 2:40:30 AM     1 11114919637
    ## 3 1503960366 3/13/2016 2:41:30 AM     1 11114919637
    ## 4 1503960366 3/13/2016 2:42:30 AM     1 11114919637
    ## 5 1503960366 3/13/2016 2:43:30 AM     1 11114919637
    ## 6 1503960366 3/13/2016 2:44:30 AM     1 11114919637

``` r
minuteStepsNarrow_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/minuteStepsNarrow_merged.csv")
```

    ## Rows: 1445040 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityMinute
    ## dbl (2): Id, Steps
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteStepsNarrow_merged1)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute        Steps
    ##        <dbl> <chr>                 <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM     0
    ## 2 1503960366 3/12/2016 12:01:00 AM     0
    ## 3 1503960366 3/12/2016 12:02:00 AM     0
    ## 4 1503960366 3/12/2016 12:03:00 AM     0
    ## 5 1503960366 3/12/2016 12:04:00 AM     0
    ## 6 1503960366 3/12/2016 12:05:00 AM     0

``` r
weightLogInfo_merged1 <- read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/weightLogInfo_merged.csv")
```

    ## Rows: 33 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Date
    ## dbl (6): Id, WeightKg, WeightPounds, Fat, BMI, LogId
    ## lgl (1): IsManualReport
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(weightLogInfo_merged1)
```

    ## # A tibble: 6 × 8
    ##           Id Date       WeightKg WeightPounds   Fat   BMI IsManualReport   LogId
    ##        <dbl> <chr>         <dbl>        <dbl> <dbl> <dbl> <lgl>            <dbl>
    ## 1 1503960366 4/5/2016 …     53.3         118.    22  23.0 TRUE           1.46e12
    ## 2 1927972279 4/10/2016…    130.          286.    NA  46.2 FALSE          1.46e12
    ## 3 2347167796 4/3/2016 …     63.4         140.    10  24.8 TRUE           1.46e12
    ## 4 2873212765 4/6/2016 …     56.7         125.    NA  21.5 TRUE           1.46e12
    ## 5 2873212765 4/7/2016 …     57.2         126.    NA  21.6 TRUE           1.46e12
    ## 6 2891001357 4/5/2016 …     88.4         195.    NA  25.0 TRUE           1.46e12

## Load and preview datasets from 4.12.16-5.12.16 into data frames

``` r
Daily_Activity2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
```

    ## Rows: 940 Columns: 15
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityDate
    ## dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDi...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(Daily_Activity2)
```

    ## # A tibble: 6 × 15
    ##           Id ActivityDate TotalSteps TotalDistance TrackerDistance
    ##        <dbl> <chr>             <dbl>         <dbl>           <dbl>
    ## 1 1503960366 4/12/2016         13162          8.5             8.5 
    ## 2 1503960366 4/13/2016         10735          6.97            6.97
    ## 3 1503960366 4/14/2016         10460          6.74            6.74
    ## 4 1503960366 4/15/2016          9762          6.28            6.28
    ## 5 1503960366 4/16/2016         12669          8.16            8.16
    ## 6 1503960366 4/17/2016          9705          6.48            6.48
    ## # ℹ 10 more variables: LoggedActivitiesDistance <dbl>,
    ## #   VeryActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
    ## #   LightActiveDistance <dbl>, SedentaryActiveDistance <dbl>,
    ## #   VeryActiveMinutes <dbl>, FairlyActiveMinutes <dbl>,
    ## #   LightlyActiveMinutes <dbl>, SedentaryMinutes <dbl>, Calories <dbl>

``` r
dailyCalories_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/dailyCalories_merged.csv")
```

    ## Rows: 940 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityDay
    ## dbl (2): Id, Calories
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(dailyCalories_merged2)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityDay Calories
    ##        <dbl> <chr>          <dbl>
    ## 1 1503960366 4/12/2016       1985
    ## 2 1503960366 4/13/2016       1797
    ## 3 1503960366 4/14/2016       1776
    ## 4 1503960366 4/15/2016       1745
    ## 5 1503960366 4/16/2016       1863
    ## 6 1503960366 4/17/2016       1728

``` r
dailyIntensities_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/dailyIntensities_merged.csv")
```

    ## Rows: 940 Columns: 10
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityDay
    ## dbl (9): Id, SedentaryMinutes, LightlyActiveMinutes, FairlyActiveMinutes, Ve...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(dailyIntensities_merged2)
```

    ## # A tibble: 6 × 10
    ##         Id ActivityDay SedentaryMinutes LightlyActiveMinutes FairlyActiveMinutes
    ##      <dbl> <chr>                  <dbl>                <dbl>               <dbl>
    ## 1   1.50e9 4/12/2016                728                  328                  13
    ## 2   1.50e9 4/13/2016                776                  217                  19
    ## 3   1.50e9 4/14/2016               1218                  181                  11
    ## 4   1.50e9 4/15/2016                726                  209                  34
    ## 5   1.50e9 4/16/2016                773                  221                  10
    ## 6   1.50e9 4/17/2016                539                  164                  20
    ## # ℹ 5 more variables: VeryActiveMinutes <dbl>, SedentaryActiveDistance <dbl>,
    ## #   LightActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
    ## #   VeryActiveDistance <dbl>

``` r
dailySteps_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/dailySteps_merged.csv")
```

    ## Rows: 940 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityDay
    ## dbl (2): Id, StepTotal
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(dailySteps_merged2)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityDay StepTotal
    ##        <dbl> <chr>           <dbl>
    ## 1 1503960366 4/12/2016       13162
    ## 2 1503960366 4/13/2016       10735
    ## 3 1503960366 4/14/2016       10460
    ## 4 1503960366 4/15/2016        9762
    ## 5 1503960366 4/16/2016       12669
    ## 6 1503960366 4/17/2016        9705

``` r
heartrate_seconds_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/heartrate_seconds_merged.csv")
```

    ## Rows: 2483658 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Time
    ## dbl (2): Id, Value
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(heartrate_seconds_merged2)
```

    ## # A tibble: 6 × 3
    ##           Id Time                 Value
    ##        <dbl> <chr>                <dbl>
    ## 1 2022484408 4/12/2016 7:21:00 AM    97
    ## 2 2022484408 4/12/2016 7:21:05 AM   102
    ## 3 2022484408 4/12/2016 7:21:10 AM   105
    ## 4 2022484408 4/12/2016 7:21:20 AM   103
    ## 5 2022484408 4/12/2016 7:21:25 AM   101
    ## 6 2022484408 4/12/2016 7:22:05 AM    95

``` r
hourlyCalories_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/hourlyCalories_merged.csv")
```

    ## Rows: 22099 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (2): Id, Calories
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(hourlyCalories_merged2)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityHour          Calories
    ##        <dbl> <chr>                    <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM       81
    ## 2 1503960366 4/12/2016 1:00:00 AM        61
    ## 3 1503960366 4/12/2016 2:00:00 AM        59
    ## 4 1503960366 4/12/2016 3:00:00 AM        47
    ## 5 1503960366 4/12/2016 4:00:00 AM        48
    ## 6 1503960366 4/12/2016 5:00:00 AM        48

``` r
hourlyIntensities_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/hourlyIntensities_merged.csv")
```

    ## Rows: 22099 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (3): Id, TotalIntensity, AverageIntensity
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(hourlyIntensities_merged2)
```

    ## # A tibble: 6 × 4
    ##           Id ActivityHour          TotalIntensity AverageIntensity
    ##        <dbl> <chr>                          <dbl>            <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM             20            0.333
    ## 2 1503960366 4/12/2016 1:00:00 AM               8            0.133
    ## 3 1503960366 4/12/2016 2:00:00 AM               7            0.117
    ## 4 1503960366 4/12/2016 3:00:00 AM               0            0    
    ## 5 1503960366 4/12/2016 4:00:00 AM               0            0    
    ## 6 1503960366 4/12/2016 5:00:00 AM               0            0

``` r
hourlySteps_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/hourlySteps_merged.csv")
```

    ## Rows: 22099 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityHour
    ## dbl (2): Id, StepTotal
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(hourlySteps_merged2)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityHour          StepTotal
    ##        <dbl> <chr>                     <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM       373
    ## 2 1503960366 4/12/2016 1:00:00 AM        160
    ## 3 1503960366 4/12/2016 2:00:00 AM        151
    ## 4 1503960366 4/12/2016 3:00:00 AM          0
    ## 5 1503960366 4/12/2016 4:00:00 AM          0
    ## 6 1503960366 4/12/2016 5:00:00 AM          0

``` r
minuteCaloriesNarrow_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteCaloriesNarrow_merged.csv")
```

    ## Rows: 1325580 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityMinute
    ## dbl (2): Id, Calories
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteCaloriesNarrow_merged2)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute        Calories
    ##        <dbl> <chr>                    <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM    0.786
    ## 2 1503960366 4/12/2016 12:01:00 AM    0.786
    ## 3 1503960366 4/12/2016 12:02:00 AM    0.786
    ## 4 1503960366 4/12/2016 12:03:00 AM    0.786
    ## 5 1503960366 4/12/2016 12:04:00 AM    0.786
    ## 6 1503960366 4/12/2016 12:05:00 AM    0.944

``` r
minuteCaloriesWide_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteCaloriesWide_merged.csv")
```

    ## Rows: 21645 Columns: 62
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityHour
    ## dbl (61): Id, Calories00, Calories01, Calories02, Calories03, Calories04, Ca...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteCaloriesWide_merged2)
```

    ## # A tibble: 6 × 62
    ##           Id ActivityHour Calories00 Calories01 Calories02 Calories03 Calories04
    ##        <dbl> <chr>             <dbl>      <dbl>      <dbl>      <dbl>      <dbl>
    ## 1 1503960366 4/13/2016 1…      1.89       2.20       0.944      0.944      0.944
    ## 2 1503960366 4/13/2016 1…      0.786      0.786      0.786      0.786      0.944
    ## 3 1503960366 4/13/2016 2…      0.786      0.786      0.786      0.786      0.786
    ## 4 1503960366 4/13/2016 3…      0.786      0.786      0.786      0.786      0.786
    ## 5 1503960366 4/13/2016 4…      0.786      0.786      0.786      0.786      0.786
    ## 6 1503960366 4/13/2016 5…      0.786      0.786      0.786      0.786      0.786
    ## # ℹ 55 more variables: Calories05 <dbl>, Calories06 <dbl>, Calories07 <dbl>,
    ## #   Calories08 <dbl>, Calories09 <dbl>, Calories10 <dbl>, Calories11 <dbl>,
    ## #   Calories12 <dbl>, Calories13 <dbl>, Calories14 <dbl>, Calories15 <dbl>,
    ## #   Calories16 <dbl>, Calories17 <dbl>, Calories18 <dbl>, Calories19 <dbl>,
    ## #   Calories20 <dbl>, Calories21 <dbl>, Calories22 <dbl>, Calories23 <dbl>,
    ## #   Calories24 <dbl>, Calories25 <dbl>, Calories26 <dbl>, Calories27 <dbl>,
    ## #   Calories28 <dbl>, Calories29 <dbl>, Calories30 <dbl>, Calories31 <dbl>, …

``` r
minuteIntensitiesNarrow_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteIntensitiesNarrow_merged.csv")
```

    ## Rows: 1325580 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityMinute
    ## dbl (2): Id, Intensity
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteIntensitiesNarrow_merged2)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute        Intensity
    ##        <dbl> <chr>                     <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM         0
    ## 2 1503960366 4/12/2016 12:01:00 AM         0
    ## 3 1503960366 4/12/2016 12:02:00 AM         0
    ## 4 1503960366 4/12/2016 12:03:00 AM         0
    ## 5 1503960366 4/12/2016 12:04:00 AM         0
    ## 6 1503960366 4/12/2016 12:05:00 AM         0

``` r
minuteIntensitiesWide_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteIntensitiesWide_merged.csv")
```

    ## Rows: 21645 Columns: 62
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityHour
    ## dbl (61): Id, Intensity00, Intensity01, Intensity02, Intensity03, Intensity0...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteIntensitiesWide_merged2)
```

    ## # A tibble: 6 × 62
    ##           Id ActivityHour        Intensity00 Intensity01 Intensity02 Intensity03
    ##        <dbl> <chr>                     <dbl>       <dbl>       <dbl>       <dbl>
    ## 1 1503960366 4/13/2016 12:00:00…           1           1           0           0
    ## 2 1503960366 4/13/2016 1:00:00 …           0           0           0           0
    ## 3 1503960366 4/13/2016 2:00:00 …           0           0           0           0
    ## 4 1503960366 4/13/2016 3:00:00 …           0           0           0           0
    ## 5 1503960366 4/13/2016 4:00:00 …           0           0           0           0
    ## 6 1503960366 4/13/2016 5:00:00 …           0           0           0           0
    ## # ℹ 56 more variables: Intensity04 <dbl>, Intensity05 <dbl>, Intensity06 <dbl>,
    ## #   Intensity07 <dbl>, Intensity08 <dbl>, Intensity09 <dbl>, Intensity10 <dbl>,
    ## #   Intensity11 <dbl>, Intensity12 <dbl>, Intensity13 <dbl>, Intensity14 <dbl>,
    ## #   Intensity15 <dbl>, Intensity16 <dbl>, Intensity17 <dbl>, Intensity18 <dbl>,
    ## #   Intensity19 <dbl>, Intensity20 <dbl>, Intensity21 <dbl>, Intensity22 <dbl>,
    ## #   Intensity23 <dbl>, Intensity24 <dbl>, Intensity25 <dbl>, Intensity26 <dbl>,
    ## #   Intensity27 <dbl>, Intensity28 <dbl>, Intensity29 <dbl>, …

``` r
minuteMETsNarrow_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteMETsNarrow_merged.csv")
```

    ## Rows: 1325580 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityMinute
    ## dbl (2): Id, METs
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteMETsNarrow_merged2)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute         METs
    ##        <dbl> <chr>                 <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM    10
    ## 2 1503960366 4/12/2016 12:01:00 AM    10
    ## 3 1503960366 4/12/2016 12:02:00 AM    10
    ## 4 1503960366 4/12/2016 12:03:00 AM    10
    ## 5 1503960366 4/12/2016 12:04:00 AM    10
    ## 6 1503960366 4/12/2016 12:05:00 AM    12

``` r
minuteSleep_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteSleep_merged.csv")
```

    ## Rows: 188521 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): date
    ## dbl (3): Id, value, logId
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteSleep_merged2)
```

    ## # A tibble: 6 × 4
    ##           Id date                 value       logId
    ##        <dbl> <chr>                <dbl>       <dbl>
    ## 1 1503960366 4/12/2016 2:47:30 AM     3 11380564589
    ## 2 1503960366 4/12/2016 2:48:30 AM     2 11380564589
    ## 3 1503960366 4/12/2016 2:49:30 AM     1 11380564589
    ## 4 1503960366 4/12/2016 2:50:30 AM     1 11380564589
    ## 5 1503960366 4/12/2016 2:51:30 AM     1 11380564589
    ## 6 1503960366 4/12/2016 2:52:30 AM     1 11380564589

``` r
minuteStepsNarrow_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteStepsNarrow_merged.csv")
```

    ## Rows: 1325580 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityMinute
    ## dbl (2): Id, Steps
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteStepsNarrow_merged2)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute        Steps
    ##        <dbl> <chr>                 <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM     0
    ## 2 1503960366 4/12/2016 12:01:00 AM     0
    ## 3 1503960366 4/12/2016 12:02:00 AM     0
    ## 4 1503960366 4/12/2016 12:03:00 AM     0
    ## 5 1503960366 4/12/2016 12:04:00 AM     0
    ## 6 1503960366 4/12/2016 12:05:00 AM     0

``` r
minuteStepsWide_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteStepsWide_merged.csv")
```

    ## Rows: 21645 Columns: 62
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityHour
    ## dbl (61): Id, Steps00, Steps01, Steps02, Steps03, Steps04, Steps05, Steps06,...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(minuteStepsWide_merged2)
```

    ## # A tibble: 6 × 62
    ##          Id ActivityHour Steps00 Steps01 Steps02 Steps03 Steps04 Steps05 Steps06
    ##       <dbl> <chr>          <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ## 1    1.50e9 4/13/2016 1…       4      16       0       0       0       9       0
    ## 2    1.50e9 4/13/2016 1…       0       0       0       0       0       0       0
    ## 3    1.50e9 4/13/2016 2…       0       0       0       0       0       0       0
    ## 4    1.50e9 4/13/2016 3…       0       0       0       0       0       0       0
    ## 5    1.50e9 4/13/2016 4…       0       0       0       0       0       0       0
    ## 6    1.50e9 4/13/2016 5…       0       0       0       0       0       0       0
    ## # ℹ 53 more variables: Steps07 <dbl>, Steps08 <dbl>, Steps09 <dbl>,
    ## #   Steps10 <dbl>, Steps11 <dbl>, Steps12 <dbl>, Steps13 <dbl>, Steps14 <dbl>,
    ## #   Steps15 <dbl>, Steps16 <dbl>, Steps17 <dbl>, Steps18 <dbl>, Steps19 <dbl>,
    ## #   Steps20 <dbl>, Steps21 <dbl>, Steps22 <dbl>, Steps23 <dbl>, Steps24 <dbl>,
    ## #   Steps25 <dbl>, Steps26 <dbl>, Steps27 <dbl>, Steps28 <dbl>, Steps29 <dbl>,
    ## #   Steps30 <dbl>, Steps31 <dbl>, Steps32 <dbl>, Steps33 <dbl>, Steps34 <dbl>,
    ## #   Steps35 <dbl>, Steps36 <dbl>, Steps37 <dbl>, Steps38 <dbl>, …

``` r
sleepDay_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
```

    ## Rows: 413 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): SleepDay
    ## dbl (4): Id, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(sleepDay_merged2)
```

    ## # A tibble: 6 × 5
    ##           Id SleepDay        TotalSleepRecords TotalMinutesAsleep TotalTimeInBed
    ##        <dbl> <chr>                       <dbl>              <dbl>          <dbl>
    ## 1 1503960366 4/12/2016 12:0…                 1                327            346
    ## 2 1503960366 4/13/2016 12:0…                 2                384            407
    ## 3 1503960366 4/15/2016 12:0…                 1                412            442
    ## 4 1503960366 4/16/2016 12:0…                 2                340            367
    ## 5 1503960366 4/17/2016 12:0…                 1                700            712
    ## 6 1503960366 4/19/2016 12:0…                 1                304            320

``` r
weightLogInfo_merged2 <- read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv")
```

    ## Rows: 67 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Date
    ## dbl (6): Id, WeightKg, WeightPounds, Fat, BMI, LogId
    ## lgl (1): IsManualReport
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(weightLogInfo_merged2)
```

    ## # A tibble: 6 × 8
    ##           Id Date       WeightKg WeightPounds   Fat   BMI IsManualReport   LogId
    ##        <dbl> <chr>         <dbl>        <dbl> <dbl> <dbl> <lgl>            <dbl>
    ## 1 1503960366 5/2/2016 …     52.6         116.    22  22.6 TRUE           1.46e12
    ## 2 1503960366 5/3/2016 …     52.6         116.    NA  22.6 TRUE           1.46e12
    ## 3 1927972279 4/13/2016…    134.          294.    NA  47.5 FALSE          1.46e12
    ## 4 2873212765 4/21/2016…     56.7         125.    NA  21.5 TRUE           1.46e12
    ## 5 2873212765 5/12/2016…     57.3         126.    NA  21.7 TRUE           1.46e12
    ## 6 4319703577 4/17/2016…     72.4         160.    25  27.5 TRUE           1.46e12

## bind together related tables and sort by ID

``` r
daily_Activity_merged <- rbind(daily_Activity_merged1, Daily_Activity2) %>%
  arrange(Id)
head(daily_Activity_merged)
```

    ## # A tibble: 6 × 15
    ##           Id ActivityDate TotalSteps TotalDistance TrackerDistance
    ##        <dbl> <chr>             <dbl>         <dbl>           <dbl>
    ## 1 1503960366 3/25/2016         11004          7.11            7.11
    ## 2 1503960366 3/26/2016         17609         11.6            11.6 
    ## 3 1503960366 3/27/2016         12736          8.53            8.53
    ## 4 1503960366 3/28/2016         13231          8.93            8.93
    ## 5 1503960366 3/29/2016         12041          7.85            7.85
    ## 6 1503960366 3/30/2016         10970          7.16            7.16
    ## # ℹ 10 more variables: LoggedActivitiesDistance <dbl>,
    ## #   VeryActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
    ## #   LightActiveDistance <dbl>, SedentaryActiveDistance <dbl>,
    ## #   VeryActiveMinutes <dbl>, FairlyActiveMinutes <dbl>,
    ## #   LightlyActiveMinutes <dbl>, SedentaryMinutes <dbl>, Calories <dbl>

``` r
dailyCalories_merged <- dailyCalories_merged2 %>%
  arrange(Id)
head(dailyCalories_merged)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityDay Calories
    ##        <dbl> <chr>          <dbl>
    ## 1 1503960366 4/12/2016       1985
    ## 2 1503960366 4/13/2016       1797
    ## 3 1503960366 4/14/2016       1776
    ## 4 1503960366 4/15/2016       1745
    ## 5 1503960366 4/16/2016       1863
    ## 6 1503960366 4/17/2016       1728

``` r
dailyIntensities_merged <- dailyIntensities_merged2 %>%
  arrange(Id)
head(dailyIntensities_merged)
```

    ## # A tibble: 6 × 10
    ##         Id ActivityDay SedentaryMinutes LightlyActiveMinutes FairlyActiveMinutes
    ##      <dbl> <chr>                  <dbl>                <dbl>               <dbl>
    ## 1   1.50e9 4/12/2016                728                  328                  13
    ## 2   1.50e9 4/13/2016                776                  217                  19
    ## 3   1.50e9 4/14/2016               1218                  181                  11
    ## 4   1.50e9 4/15/2016                726                  209                  34
    ## 5   1.50e9 4/16/2016                773                  221                  10
    ## 6   1.50e9 4/17/2016                539                  164                  20
    ## # ℹ 5 more variables: VeryActiveMinutes <dbl>, SedentaryActiveDistance <dbl>,
    ## #   LightActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
    ## #   VeryActiveDistance <dbl>

``` r
dailySteps_merged <- dailySteps_merged2 %>%
  arrange(Id)
head(dailySteps_merged)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityDay StepTotal
    ##        <dbl> <chr>           <dbl>
    ## 1 1503960366 4/12/2016       13162
    ## 2 1503960366 4/13/2016       10735
    ## 3 1503960366 4/14/2016       10460
    ## 4 1503960366 4/15/2016        9762
    ## 5 1503960366 4/16/2016       12669
    ## 6 1503960366 4/17/2016        9705

``` r
heartrate_seconds_merged <- rbind(heartrate_seconds_merged1, heartrate_seconds_merged2) %>%
  arrange(Id)
head(heartrate_seconds_merged)
```

    ## # A tibble: 6 × 3
    ##           Id Time                Value
    ##        <dbl> <chr>               <dbl>
    ## 1 2022484408 4/1/2016 7:54:00 AM    93
    ## 2 2022484408 4/1/2016 7:54:05 AM    91
    ## 3 2022484408 4/1/2016 7:54:10 AM    96
    ## 4 2022484408 4/1/2016 7:54:15 AM    98
    ## 5 2022484408 4/1/2016 7:54:20 AM   100
    ## 6 2022484408 4/1/2016 7:54:25 AM   101

``` r
hourlyCalories_merged <-rbind(hourlyCalories_merged1, hourlyCalories_merged2) %>%
  arrange(Id)
head(hourlyCalories_merged)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityHour          Calories
    ##        <dbl> <chr>                    <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM       48
    ## 2 1503960366 3/12/2016 1:00:00 AM        48
    ## 3 1503960366 3/12/2016 2:00:00 AM        48
    ## 4 1503960366 3/12/2016 3:00:00 AM        48
    ## 5 1503960366 3/12/2016 4:00:00 AM        48
    ## 6 1503960366 3/12/2016 5:00:00 AM        48

``` r
hourlyIntensities_merged <- rbind(hourlyIntensities_merged1, hourlyIntensities_merged2) %>%
  arrange(Id)
head(hourlyIntensities_merged)
```

    ## # A tibble: 6 × 4
    ##           Id ActivityHour          TotalIntensity AverageIntensity
    ##        <dbl> <chr>                          <dbl>            <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM              0                0
    ## 2 1503960366 3/12/2016 1:00:00 AM               0                0
    ## 3 1503960366 3/12/2016 2:00:00 AM               0                0
    ## 4 1503960366 3/12/2016 3:00:00 AM               0                0
    ## 5 1503960366 3/12/2016 4:00:00 AM               0                0
    ## 6 1503960366 3/12/2016 5:00:00 AM               0                0

``` r
hourlySteps_merged <- rbind(hourlySteps_merged1, hourlySteps_merged2) %>%
  arrange(Id)
head(hourlySteps_merged)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityHour          StepTotal
    ##        <dbl> <chr>                     <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM         0
    ## 2 1503960366 3/12/2016 1:00:00 AM          0
    ## 3 1503960366 3/12/2016 2:00:00 AM          0
    ## 4 1503960366 3/12/2016 3:00:00 AM          0
    ## 5 1503960366 3/12/2016 4:00:00 AM          0
    ## 6 1503960366 3/12/2016 5:00:00 AM          0

``` r
minuteCaloriesNarrow_merged <- rbind(minuteCaloriesNarrow_merged1, minuteCaloriesNarrow_merged2) %>%
  arrange(Id)
head(minuteCaloriesNarrow_merged)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute        Calories
    ##        <dbl> <chr>                    <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM    0.797
    ## 2 1503960366 3/12/2016 12:01:00 AM    0.797
    ## 3 1503960366 3/12/2016 12:02:00 AM    0.797
    ## 4 1503960366 3/12/2016 12:03:00 AM    0.797
    ## 5 1503960366 3/12/2016 12:04:00 AM    0.797
    ## 6 1503960366 3/12/2016 12:05:00 AM    0.797

``` r
minuteCaloriesWide_merged <- minuteCaloriesWide_merged2 %>%
  arrange(Id)
head(minuteCaloriesWide_merged)
```

    ## # A tibble: 6 × 62
    ##           Id ActivityHour Calories00 Calories01 Calories02 Calories03 Calories04
    ##        <dbl> <chr>             <dbl>      <dbl>      <dbl>      <dbl>      <dbl>
    ## 1 1503960366 4/13/2016 1…      1.89       2.20       0.944      0.944      0.944
    ## 2 1503960366 4/13/2016 1…      0.786      0.786      0.786      0.786      0.944
    ## 3 1503960366 4/13/2016 2…      0.786      0.786      0.786      0.786      0.786
    ## 4 1503960366 4/13/2016 3…      0.786      0.786      0.786      0.786      0.786
    ## 5 1503960366 4/13/2016 4…      0.786      0.786      0.786      0.786      0.786
    ## 6 1503960366 4/13/2016 5…      0.786      0.786      0.786      0.786      0.786
    ## # ℹ 55 more variables: Calories05 <dbl>, Calories06 <dbl>, Calories07 <dbl>,
    ## #   Calories08 <dbl>, Calories09 <dbl>, Calories10 <dbl>, Calories11 <dbl>,
    ## #   Calories12 <dbl>, Calories13 <dbl>, Calories14 <dbl>, Calories15 <dbl>,
    ## #   Calories16 <dbl>, Calories17 <dbl>, Calories18 <dbl>, Calories19 <dbl>,
    ## #   Calories20 <dbl>, Calories21 <dbl>, Calories22 <dbl>, Calories23 <dbl>,
    ## #   Calories24 <dbl>, Calories25 <dbl>, Calories26 <dbl>, Calories27 <dbl>,
    ## #   Calories28 <dbl>, Calories29 <dbl>, Calories30 <dbl>, Calories31 <dbl>, …

``` r
minuteIntensitiesNarrow_merged <- rbind(minuteIntensitiesNarrow_merged1, minuteIntensitiesNarrow_merged2) %>%
  arrange(Id)
head(minuteIntensitiesNarrow_merged)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute        Intensity
    ##        <dbl> <chr>                     <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM         0
    ## 2 1503960366 3/12/2016 12:01:00 AM         0
    ## 3 1503960366 3/12/2016 12:02:00 AM         0
    ## 4 1503960366 3/12/2016 12:03:00 AM         0
    ## 5 1503960366 3/12/2016 12:04:00 AM         0
    ## 6 1503960366 3/12/2016 12:05:00 AM         0

``` r
minuteIntensitiesWide_merged<- minuteIntensitiesWide_merged2 %>%
  arrange(Id)
head(minuteIntensitiesWide_merged)
```

    ## # A tibble: 6 × 62
    ##           Id ActivityHour        Intensity00 Intensity01 Intensity02 Intensity03
    ##        <dbl> <chr>                     <dbl>       <dbl>       <dbl>       <dbl>
    ## 1 1503960366 4/13/2016 12:00:00…           1           1           0           0
    ## 2 1503960366 4/13/2016 1:00:00 …           0           0           0           0
    ## 3 1503960366 4/13/2016 2:00:00 …           0           0           0           0
    ## 4 1503960366 4/13/2016 3:00:00 …           0           0           0           0
    ## 5 1503960366 4/13/2016 4:00:00 …           0           0           0           0
    ## 6 1503960366 4/13/2016 5:00:00 …           0           0           0           0
    ## # ℹ 56 more variables: Intensity04 <dbl>, Intensity05 <dbl>, Intensity06 <dbl>,
    ## #   Intensity07 <dbl>, Intensity08 <dbl>, Intensity09 <dbl>, Intensity10 <dbl>,
    ## #   Intensity11 <dbl>, Intensity12 <dbl>, Intensity13 <dbl>, Intensity14 <dbl>,
    ## #   Intensity15 <dbl>, Intensity16 <dbl>, Intensity17 <dbl>, Intensity18 <dbl>,
    ## #   Intensity19 <dbl>, Intensity20 <dbl>, Intensity21 <dbl>, Intensity22 <dbl>,
    ## #   Intensity23 <dbl>, Intensity24 <dbl>, Intensity25 <dbl>, Intensity26 <dbl>,
    ## #   Intensity27 <dbl>, Intensity28 <dbl>, Intensity29 <dbl>, …

``` r
minuteMETsNarrow_merged <- rbind(minuteMETsNarrow_merged1, minuteMETsNarrow_merged2) %>%
  arrange(Id)
head(minuteMETsNarrow_merged)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute         METs
    ##        <dbl> <chr>                 <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM    10
    ## 2 1503960366 3/12/2016 12:01:00 AM    10
    ## 3 1503960366 3/12/2016 12:02:00 AM    10
    ## 4 1503960366 3/12/2016 12:03:00 AM    10
    ## 5 1503960366 3/12/2016 12:04:00 AM    10
    ## 6 1503960366 3/12/2016 12:05:00 AM    10

``` r
minuteSleep_merged <- rbind(minuteSleep_merged1, minuteSleep_merged2) %>%
  arrange(Id)
head(minuteSleep_merged)
```

    ## # A tibble: 6 × 4
    ##           Id date                 value       logId
    ##        <dbl> <chr>                <dbl>       <dbl>
    ## 1 1503960366 3/13/2016 2:39:30 AM     1 11114919637
    ## 2 1503960366 3/13/2016 2:40:30 AM     1 11114919637
    ## 3 1503960366 3/13/2016 2:41:30 AM     1 11114919637
    ## 4 1503960366 3/13/2016 2:42:30 AM     1 11114919637
    ## 5 1503960366 3/13/2016 2:43:30 AM     1 11114919637
    ## 6 1503960366 3/13/2016 2:44:30 AM     1 11114919637

``` r
minuteStepsNarrow_merged <- rbind(minuteStepsNarrow_merged1, minuteStepsNarrow_merged2) %>%
  arrange(Id)
head(minuteStepsNarrow_merged)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityMinute        Steps
    ##        <dbl> <chr>                 <dbl>
    ## 1 1503960366 3/12/2016 12:00:00 AM     0
    ## 2 1503960366 3/12/2016 12:01:00 AM     0
    ## 3 1503960366 3/12/2016 12:02:00 AM     0
    ## 4 1503960366 3/12/2016 12:03:00 AM     0
    ## 5 1503960366 3/12/2016 12:04:00 AM     0
    ## 6 1503960366 3/12/2016 12:05:00 AM     0

``` r
minuteStepsWide_merged <- minuteStepsWide_merged2 %>%
  arrange(Id)
head(minuteStepsWide_merged)
```

    ## # A tibble: 6 × 62
    ##          Id ActivityHour Steps00 Steps01 Steps02 Steps03 Steps04 Steps05 Steps06
    ##       <dbl> <chr>          <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ## 1    1.50e9 4/13/2016 1…       4      16       0       0       0       9       0
    ## 2    1.50e9 4/13/2016 1…       0       0       0       0       0       0       0
    ## 3    1.50e9 4/13/2016 2…       0       0       0       0       0       0       0
    ## 4    1.50e9 4/13/2016 3…       0       0       0       0       0       0       0
    ## 5    1.50e9 4/13/2016 4…       0       0       0       0       0       0       0
    ## 6    1.50e9 4/13/2016 5…       0       0       0       0       0       0       0
    ## # ℹ 53 more variables: Steps07 <dbl>, Steps08 <dbl>, Steps09 <dbl>,
    ## #   Steps10 <dbl>, Steps11 <dbl>, Steps12 <dbl>, Steps13 <dbl>, Steps14 <dbl>,
    ## #   Steps15 <dbl>, Steps16 <dbl>, Steps17 <dbl>, Steps18 <dbl>, Steps19 <dbl>,
    ## #   Steps20 <dbl>, Steps21 <dbl>, Steps22 <dbl>, Steps23 <dbl>, Steps24 <dbl>,
    ## #   Steps25 <dbl>, Steps26 <dbl>, Steps27 <dbl>, Steps28 <dbl>, Steps29 <dbl>,
    ## #   Steps30 <dbl>, Steps31 <dbl>, Steps32 <dbl>, Steps33 <dbl>, Steps34 <dbl>,
    ## #   Steps35 <dbl>, Steps36 <dbl>, Steps37 <dbl>, Steps38 <dbl>, …

``` r
sleepDay_merged <- sleepDay_merged2 %>%
  arrange(Id)
head(sleepDay_merged)
```

    ## # A tibble: 6 × 5
    ##           Id SleepDay        TotalSleepRecords TotalMinutesAsleep TotalTimeInBed
    ##        <dbl> <chr>                       <dbl>              <dbl>          <dbl>
    ## 1 1503960366 4/12/2016 12:0…                 1                327            346
    ## 2 1503960366 4/13/2016 12:0…                 2                384            407
    ## 3 1503960366 4/15/2016 12:0…                 1                412            442
    ## 4 1503960366 4/16/2016 12:0…                 2                340            367
    ## 5 1503960366 4/17/2016 12:0…                 1                700            712
    ## 6 1503960366 4/19/2016 12:0…                 1                304            320

``` r
weightLogInfo_merged <- rbind(weightLogInfo_merged1, weightLogInfo_merged2) %>%
  arrange(Id)
head(weightLogInfo_merged)
```

    ## # A tibble: 6 × 8
    ##           Id Date       WeightKg WeightPounds   Fat   BMI IsManualReport   LogId
    ##        <dbl> <chr>         <dbl>        <dbl> <dbl> <dbl> <lgl>            <dbl>
    ## 1 1503960366 4/5/2016 …     53.3         118.    22  23.0 TRUE           1.46e12
    ## 2 1503960366 5/2/2016 …     52.6         116.    22  22.6 TRUE           1.46e12
    ## 3 1503960366 5/3/2016 …     52.6         116.    NA  22.6 TRUE           1.46e12
    ## 4 1927972279 4/10/2016…    130.          286.    NA  46.2 FALSE          1.46e12
    ## 5 1927972279 4/13/2016…    134.          294.    NA  47.5 FALSE          1.46e12
    ## 6 2347167796 4/3/2016 …     63.4         140.    10  24.8 TRUE           1.46e12

# **PHASE 3: PROCESS**

## Clear out any values that are less than or equal to zero

``` r
daily_Activity_merged_no_zero <- daily_Activity_merged %>% 
  filter(TotalSteps > 0 & TotalDistance > 0 & TrackerDistance > 0 & LoggedActivitiesDistance > 0 & Calories >0)

dailyCalories_merged_no_zero <- dailyCalories_merged %>% 
  filter(Calories > 0)

dailyIntensities_merged_no_zero <- dailyIntensities_merged %>% 
  filter(VeryActiveDistance > 0 | ModeratelyActiveDistance > 0 | LightActiveDistance > 0 | SedentaryActiveDistance > 0) %>% 
  filter(VeryActiveMinutes > 0 | FairlyActiveMinutes > 0 | LightlyActiveMinutes > 0 | SedentaryMinutes > 0)

dailySteps_merged_no_zero <- dailySteps_merged %>% 
  filter(StepTotal > 0)

heartrate_seconds_merged_no_zero <- heartrate_seconds_merged %>% 
  filter(Value > 0)

hourlyCalories_merged_no_zero <- hourlyCalories_merged %>% 
  filter(Calories > 0)

hourlyIntensities_merged_no_zero <- hourlyIntensities_merged %>% 
  filter(TotalIntensity > 0 & AverageIntensity > 0)

hourlySteps_merged_no_zero <- hourlySteps_merged %>% 
  filter(StepTotal > 0)

minuteCaloriesNarrow_merged_no_zero <- minuteCaloriesNarrow_merged %>% 
  filter(Calories > 0)

minuteCaloriesWide_merged_no_zero <- minuteCaloriesWide_merged %>% 
  filter(Calories01 > 0 | Calories02 > 0 | Calories03 > 0 | Calories04 > 0 | Calories05 > 0 | Calories06 > 0 | Calories07 > 0 | Calories08 > 0 | Calories09 > 0 | Calories10 > 0 | Calories11 > 0 | Calories12 > 0 | Calories13 > 0 | Calories14 > 0 | Calories15 > 0 | Calories16 > 0 | Calories17 > 0 | Calories18 > 0 | Calories19 > 0 | Calories20 > 0 | Calories21 > 0 | Calories22 > 0 | Calories23 > 0 | Calories24 > 0 | Calories25 > 0 | Calories26 > 0 | Calories27 > 0 | Calories28 > 0 | Calories29 > 0 | Calories30 > 0 | Calories31 > 0 | Calories32 > 0 | Calories33 > 0 | Calories34 > 0 | Calories35 > 0 | Calories36 > 0 | Calories37 > 0 | Calories38 > 0 | Calories39 > 0 | Calories40 > 0 | Calories41 > 0 | Calories42 > 0 | Calories43 > 0 | Calories44 > 0 | Calories45 > 0 | Calories46 > 0 | Calories47 > 0)

minuteIntensitiesNarrow_merged_no_zero <- minuteIntensitiesNarrow_merged %>% 
  filter(Intensity > 0)

minuteIntensitiesWide_merged_no_zero <- minuteIntensitiesWide_merged %>% 
  filter(Intensity01 > 0 | Intensity02 > 0 | Intensity03 > 0 | Intensity04 > 0 | Intensity05 > 0 | Intensity06 > 0 | Intensity07 > 0 | Intensity08 > 0 | Intensity09 > 0 | Intensity10 > 0 | Intensity11 > 0 | Intensity12 > 0 | Intensity13 > 0 | Intensity14 > 0 | Intensity15 > 0 | Intensity16 > 0 | Intensity17 > 0 | Intensity18 > 0 | Intensity19 > 0 | Intensity20 > 0 | Intensity21 > 0 | Intensity22 > 0 | Intensity23 > 0 | Intensity24 > 0 | Intensity25 > 0 | Intensity26 > 0 | Intensity27 > 0 | Intensity28 > 0 | Intensity29 > 0 | Intensity30 > 0 | Intensity31 > 0 | Intensity32 > 0 | Intensity33 > 0 | Intensity34 > 0 | Intensity35 > 0 | Intensity36 > 0 | Intensity37 > 0 | Intensity38 > 0 | Intensity39 > 0 | Intensity40 > 0 | Intensity41 > 0 | Intensity42 > 0 | Intensity43 > 0 | Intensity44 > 0 | Intensity45 > 0 | Intensity46 > 0 | Intensity47 > 0)

minuteMETsNarrow_merged_no_zero <- minuteMETsNarrow_merged %>% 
  filter(METs > 0)

minuteSleep_merged_no_zero <- minuteSleep_merged %>% 
  filter(value > 0)

minuteStepsNarrow_merged_no_zero <- minuteStepsNarrow_merged %>% 
  filter(Steps > 0)

minuteStepsWide_merged_no_zero <- minuteStepsWide_merged %>% 
  filter(Steps01 > 0 | Steps02 > 0 | Steps03 > 0 | Steps04 > 0 | Steps05 > 0 | Steps06 > 0 | Steps07 > 0 | Steps08 > 0 | Steps09 > 0 | Steps10 > 0 | Steps11 > 0 | Steps12 > 0 | Steps13 > 0 | Steps14 > 0 | Steps15 > 0 | Steps16 > 0 | Steps17 > 0 | Steps18 > 0 | Steps19 > 0 | Steps20 > 0 | Steps21 > 0 | Steps22 > 0 | Steps23 > 0 | Steps24 > 0 | Steps25 > 0 | Steps26 > 0 | Steps27 > 0 | Steps28 > 0 | Steps29 > 0 | Steps30 > 0 | Steps31 > 0 | Steps32 > 0 | Steps33 > 0 | Steps34 > 0 | Steps35 > 0 | Steps36 > 0 | Steps37 > 0 | Steps38 > 0 | Steps39 > 0 | Steps40 > 0 | Steps41 > 0 | Steps42 > 0 | Steps43 > 0 | Steps44 > 0 | Steps45 > 0 | Steps46 > 0 | Steps47 > 0)

sleepDay_merged_no_zero <- sleepDay_merged %>% 
  filter(TotalSleepRecords > 0 & TotalMinutesAsleep > 0 & TotalTimeInBed > 0)

weightLogInfo_merged_no_zero <- weightLogInfo_merged %>% 
  filter(WeightKg > 0 & WeightPounds > 0 & BMI > 0)
```

## Set value in minute sleep should be 1

Value in minuteSleep_merged table should be 1 (You can literally only
sleep for 1 minute “in a minute”)

``` r
minuteSleep_merged_no_zero$value[minuteSleep_merged_no_zero$value != 1] <- 1
```

## Standardise clean dataframe names

``` r
daily_Activity_cleaned <- daily_Activity_merged_no_zero
minuteSleep_cleaned <- minuteSleep_merged_no_zero
dailyCalories_cleaned <- dailyCalories_merged_no_zero
dailyIntensities_cleaned <- dailyIntensities_merged_no_zero
dailySteps_cleaned <- dailySteps_merged_no_zero
heartrate_seconds_cleaned <- heartrate_seconds_merged_no_zero
hourlyCalories_cleaned <- hourlyCalories_merged_no_zero
hourlyIntensities_cleaned <- hourlyIntensities_merged_no_zero
hourlySteps_cleaned <- hourlySteps_merged_no_zero
minuteCaloriesNarrow_cleaned <- minuteCaloriesNarrow_merged_no_zero
minuteCaloriesWide_cleaned <- minuteCaloriesWide_merged_no_zero
minuteIntensitiesNarrow_cleaned <- minuteIntensitiesNarrow_merged_no_zero
minuteIntensitiesWide_cleaned <- minuteIntensitiesWide_merged_no_zero
minuteMETsNarrow_cleaned <- minuteMETsNarrow_merged_no_zero
minuteSleep_cleaned <- minuteSleep_merged_no_zero
minuteStepsNarrow_cleaned <- minuteStepsNarrow_merged_no_zero
minuteStepsWide_cleaned <- minuteStepsWide_merged_no_zero
sleepDay_cleaned <- sleepDay_merged_no_zero
weightLogInfo_cleaned <- weightLogInfo_merged_no_zero
```

## Prepare CSV files for Visualisation

``` r
write.csv(daily_Activity_cleaned, "Processing Visualisations/Data/daily_Activity_cleaned.csv")
write.csv(minuteSleep_cleaned, "Processing Visualisations/Data/minuteSleep_cleaned.csv")
write.csv(dailyCalories_cleaned, "Processing Visualisations/Data/dailyCalories_cleaned.csv")
write.csv(dailyIntensities_cleaned, "Processing Visualisations/Data/dailyIntensities_cleaned.csv")
write.csv(dailySteps_cleaned, "Processing Visualisations/Data/dailySteps_cleaned.csv")
write.csv(heartrate_seconds_cleaned, "Processing Visualisations/Data/heartrate_seconds_cleaned.csv")
write.csv(hourlyCalories_cleaned, "Processing Visualisations/Data/hourlyCalories_cleaned.csv")
write.csv(hourlyIntensities_cleaned, "Processing Visualisations/Data/hourlyIntensities_cleaned.csv")
write.csv(hourlySteps_cleaned, "Processing Visualisations/Data/hourlySteps_cleaned.csv")
write.csv(minuteCaloriesNarrow_cleaned, "Processing Visualisations/Data/minuteCaloriesNarrow_cleaned.csv")
write.csv(minuteCaloriesWide_cleaned, "Processing Visualisations/Data/minuteCaloriesWide_cleaned.csv")
write.csv(minuteIntensitiesNarrow_cleaned, "Processing Visualisations/Data/minuteIntensitiesNarrow_cleaned.csv")
write.csv(minuteIntensitiesWide_cleaned, "Processing Visualisations/Data/minuteIntensitiesWide_cleaned.csv")
write.csv(minuteMETsNarrow_cleaned, "Processing Visualisations/Data/minuteMETsNarrow_cleaned.csv")
write.csv(minuteSleep_cleaned, "Processing Visualisations/Data/minuteSleep_cleaned.csv")
write.csv(minuteStepsNarrow_cleaned, "Processing Visualisations/Data/minuteStepsNarrow_cleaned.csv")
write.csv(minuteStepsWide_cleaned, "Processing Visualisations/Data/minuteStepsWide_cleaned.csv")
write.csv(sleepDay_cleaned, "Processing Visualisations/Data/sleepDay_cleaned.csv")
write.csv(weightLogInfo_cleaned, "Processing Visualisations/Data/weightLogInfo_cleaned.csv")
```

# **PHASE 4: ANALYSE**

## Calculate Totals on Different Devices

``` r
Daily_Activity_Device_Totals <- summarise(daily_Activity_cleaned, Total_Minutes_From_daily_Activity_Device = sum(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes), Total_Distance_From_daily_Activity_Device = sum(VeryActiveDistance, ModeratelyActiveDistance, LightActiveDistance, SedentaryActiveDistance))

head(Daily_Activity_Device_Totals)
```

    ## # A tibble: 1 × 2
    ##   Total_Minutes_From_daily_Activity_Device Total_Distance_From_daily_Activity_…¹
    ##                                      <dbl>                                 <dbl>
    ## 1                                    60446                                  461.
    ## # ℹ abbreviated name: ¹​Total_Distance_From_daily_Activity_Device

``` r
Daily_Intensities_Device_Totals <- summarise(dailyIntensities_cleaned, Total_Minutes_From_daily_Intensities_Device = sum(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes), Total_Distance_From_daily_Intensities_Device = sum(VeryActiveDistance, ModeratelyActiveDistance, LightActiveDistance, SedentaryActiveDistance))

head(Daily_Intensities_Device_Totals)
```

    ## # A tibble: 1 × 2
    ##   Total_Minutes_From_daily_Intensities_Device Total_Distance_From_daily_Intens…¹
    ##                                         <dbl>                              <dbl>
    ## 1                                     1027152                              5088.
    ## # ℹ abbreviated name: ¹​Total_Distance_From_daily_Intensities_Device

## Visualise Data

![A visualisation of the relationships
created](Processing%20Visualisations/Relationships.png) \## Filter Data

After looking at the relationship map we are able to see that the most
effective and efficient course of action is to look at daily data a lot
of the other tables (hourly data, minute data, seconds data, wide data,
narrow data) are create a lot of unnecessary noise.

<figure>
<img src="Processing%20Visualisations/CleanedRelationships.png"
alt="A visualisation of the cleaned relationship map created" />
<figcaption aria-hidden="true">A visualisation of the cleaned
relationship map created</figcaption>
</figure>

Because each table represents a reading from a different BellaBeat
device and the daily_Activity table does not contain any sleep info to
be compared with the sleep info from sleepDay_cleaned we cannot compare
sleep info from different BellaBeat Devices **we can ignore the
sleepDay_cleaned**

<figure>
<img src="Processing%20Visualisations/Final%20Relationships.png"
alt="A visualisation of the final relationship map created" />
<figcaption aria-hidden="true">A visualisation of the final relationship
map created</figcaption>
</figure>

<div id="viz1745842154835" class="tableauPlaceholder"
style="position: relative">

<noscript>
<a href='#'><img alt='Comparison Of Data Captured On Different Bellabeat Devices ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Be&#47;BellaBeat_Data_Viz&#47;FinalStory&#47;1_rss.png' style='border: none' /></a>
</noscript>
<object class="tableauViz" style="display:none;">
<param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' />
<param name='embed_code_version' value='3' />
<param name='site_root' value='' /><param name='name' value='BellaBeat_Data_Viz&#47;FinalStory' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Be&#47;BellaBeat_Data_Viz&#47;FinalStory&#47;1.png' />
<param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /><param name='filter' value='publish=yes' />
</object>

</div>

``` js

var divElement = document.getElementById('viz1745842154835');                    
var vizElement = divElement.getElementsByTagName('object')[0];                    
vizElement.style.width='1169px';vizElement.style.height='854px';                    
var scriptElement = document.createElement('script');                    
scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    
vizElement.parentNode.insertBefore(scriptElement, vizElement);         
```

<script>
&#10;var divElement = document.getElementById('viz1745842154835');                    
var vizElement = divElement.getElementsByTagName('object')[0];                    
vizElement.style.width='1169px';vizElement.style.height='854px';                    
var scriptElement = document.createElement('script');                    
scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    
vizElement.parentNode.insertBefore(scriptElement, vizElement);         
&#10;</script>

## **PHASE 5: SHARE INSIGHTS**

**Insight:** Most of the minutes captured are classified as “Sedentary”
**Implication:** Customers using devices are sedentary most of the time
**Recommendation:** Develop a marketing strategy targeted for customers
who use products while sedentary (e.g for people at work or in class)

**Insight:** More records are captured on most devices during the
weekend **Implication:** Customers use devices more frequently on
weekends **Recommendation:** Develop a marketing strategy targeted for
customers who use products during the weekend during weekends

**Insight:** Device used to capture data for dailyIntensities_cleaned
captures much higher readings than the device used to capture
daily_Actitvity **Implication:** Data integrity is compromised  
**Recommendation:** Invest more time in working on solutions to ensure
data captured is accurate and consistent across different devices

**Insight:** Device used to capture data for daily_Actitvity captures
far fewer records than any of the other devices **Implication:** Data
integrity is compromised  
**Recommendation:** Invest more time in working on solutions to ensure
this device captures records more frequently to improve data accuracy
and consistency

**Insight:** Different Devices don’t have the same activity level
categories **Implication:** Different devices don’t capture data in a
consistent fashion **Recommendation:** Develop updates to ensure all
devices capture date in the same categories
