Cyclistic_Data_Analysis
================
Shima Mojapelo
2025-04-23

# PHASE 1: *ASK*

This case study is for a Chicago-based bike rental company named
**Cyclistic**. The company has 5824 bikes in its fleet spread across 692
docking stations where customers can start or end their trips.
Cyclistic’s members can be broadly broken up into 2 categories:
**casual** and **members**. The majority of customers are members.
Cyclistic’s Director of Marketing (Lily Moreno) is looking to develop
marketing campaigns and initiatives aimed to **convert casual customers
to members** and has thus tasked the data analytics team to **understand
the key differences between casual customers and members** in order to
improve the decision-making for future marketing campaigns and
initiatives. This project will need the Cyclistic team to approve any
recommendations.

# PHASE 2: *PREPARE*

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
    ##  C:\Users\shima\AppData\Local\Temp\RtmpS8h56r\downloaded_packages

``` r
install.packages("dplyr")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'dplyr' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\RtmpS8h56r\downloaded_packages

``` r
install.packages("ggplot2")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'ggplot2' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\RtmpS8h56r\downloaded_packages

``` r
install.packages("lubridate")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'lubridate' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\RtmpS8h56r\downloaded_packages

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

Divvy_Trips_2019_Q1 <- read_csv("Task A/Divvy_Trips_2019_Q1/Divvy_Trips_2019_Q1.csv")
```

    ## Rows: 365069 Columns: 12
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (4): from_station_name, to_station_name, usertype, gender
    ## dbl  (5): trip_id, bikeid, from_station_id, to_station_id, birthyear
    ## num  (1): tripduration
    ## dttm (2): start_time, end_time
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
Divvy_Trips_2020_Q1 <- read_csv("Task A/Divvy_Trips_2020_Q1/Divvy_Trips_2020_Q1.csv")
```

    ## Rows: 426887 Columns: 13
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (5): ride_id, rideable_type, start_station_name, end_station_name, memb...
    ## dbl  (6): start_station_id, end_station_id, start_lat, start_lng, end_lat, e...
    ## dttm (2): started_at, ended_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

## Preview the dataframes

``` r
str(Divvy_Trips_2019_Q1)
```

    ## spc_tbl_ [365,069 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ trip_id          : num [1:365069] 21742443 21742444 21742445 21742446 21742447 ...
    ##  $ start_time       : POSIXct[1:365069], format: "2019-01-01 00:04:37" "2019-01-01 00:08:13" ...
    ##  $ end_time         : POSIXct[1:365069], format: "2019-01-01 00:11:07" "2019-01-01 00:15:34" ...
    ##  $ bikeid           : num [1:365069] 2167 4386 1524 252 1170 ...
    ##  $ tripduration     : num [1:365069] 390 441 829 1783 364 ...
    ##  $ from_station_id  : num [1:365069] 199 44 15 123 173 98 98 211 150 268 ...
    ##  $ from_station_name: chr [1:365069] "Wabash Ave & Grand Ave" "State St & Randolph St" "Racine Ave & 18th St" "California Ave & Milwaukee Ave" ...
    ##  $ to_station_id    : num [1:365069] 84 624 644 176 35 49 49 142 148 141 ...
    ##  $ to_station_name  : chr [1:365069] "Milwaukee Ave & Grand Ave" "Dearborn St & Van Buren St (*)" "Western Ave & Fillmore St (*)" "Clark St & Elm St" ...
    ##  $ usertype         : chr [1:365069] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
    ##  $ gender           : chr [1:365069] "Male" "Female" "Female" "Male" ...
    ##  $ birthyear        : num [1:365069] 1989 1990 1994 1993 1994 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   trip_id = col_double(),
    ##   ..   start_time = col_datetime(format = ""),
    ##   ..   end_time = col_datetime(format = ""),
    ##   ..   bikeid = col_double(),
    ##   ..   tripduration = col_number(),
    ##   ..   from_station_id = col_double(),
    ##   ..   from_station_name = col_character(),
    ##   ..   to_station_id = col_double(),
    ##   ..   to_station_name = col_character(),
    ##   ..   usertype = col_character(),
    ##   ..   gender = col_character(),
    ##   ..   birthyear = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
str(Divvy_Trips_2020_Q1)
```

    ## spc_tbl_ [426,887 × 13] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ ride_id           : chr [1:426887] "EACB19130B0CDA4A" "8FED874C809DC021" "789F3C21E472CA96" "C9A388DAC6ABF313" ...
    ##  $ rideable_type     : chr [1:426887] "docked_bike" "docked_bike" "docked_bike" "docked_bike" ...
    ##  $ started_at        : POSIXct[1:426887], format: "2020-01-21 20:06:59" "2020-01-30 14:22:39" ...
    ##  $ ended_at          : POSIXct[1:426887], format: "2020-01-21 20:14:30" "2020-01-30 14:26:22" ...
    ##  $ start_station_name: chr [1:426887] "Western Ave & Leland Ave" "Clark St & Montrose Ave" "Broadway & Belmont Ave" "Clark St & Randolph St" ...
    ##  $ start_station_id  : num [1:426887] 239 234 296 51 66 212 96 96 212 38 ...
    ##  $ end_station_name  : chr [1:426887] "Clark St & Leland Ave" "Southport Ave & Irving Park Rd" "Wilton Ave & Belmont Ave" "Fairbanks Ct & Grand Ave" ...
    ##  $ end_station_id    : num [1:426887] 326 318 117 24 212 96 212 212 96 100 ...
    ##  $ start_lat         : num [1:426887] 42 42 41.9 41.9 41.9 ...
    ##  $ start_lng         : num [1:426887] -87.7 -87.7 -87.6 -87.6 -87.6 ...
    ##  $ end_lat           : num [1:426887] 42 42 41.9 41.9 41.9 ...
    ##  $ end_lng           : num [1:426887] -87.7 -87.7 -87.7 -87.6 -87.6 ...
    ##  $ member_casual     : chr [1:426887] "member" "member" "member" "member" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   ride_id = col_character(),
    ##   ..   rideable_type = col_character(),
    ##   ..   started_at = col_datetime(format = ""),
    ##   ..   ended_at = col_datetime(format = ""),
    ##   ..   start_station_name = col_character(),
    ##   ..   start_station_id = col_double(),
    ##   ..   end_station_name = col_character(),
    ##   ..   end_station_id = col_double(),
    ##   ..   start_lat = col_double(),
    ##   ..   start_lng = col_double(),
    ##   ..   end_lat = col_double(),
    ##   ..   end_lng = col_double(),
    ##   ..   member_casual = col_character()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

## Confirm no. of bikes

We need to confirm that both tables have \<= 5824 unique bikes (as per
fleet size), *(we can only confirm for the Divvy_Trips_2019 table as it
has a column for capturing bike_id)* **Count the number of unique bikes
in new_2019_Q1, and ensure there are no bikeid’s that are less than 0 or
greater than 5824**

``` r
bike_id_check_2019 <- select(Divvy_Trips_2019_Q1, bikeid) 

bike_id_check_2019 %>% 
  arrange(bikeid) %>% 
  unique() %>% 
  count() 
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1  4769

## Confirm no. of Docking Stations

We need to ensure that their are only 692 docking stations, therefore,
*we need to confirm that there are only 692 unique start & end station
names* **count the number of unique start and station names for 2019 and
2020**

``` r
station_name_check_2019 <- select(Divvy_Trips_2019_Q1, from_station_name, to_station_name) 

start_station_name_count_2019 = station_name_check_2019 %>% 
  select(from_station_name) %>% 
  unique() %>% 
  count() 
start_station_name_count_2019
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1   594

``` r
end_station_name_count_2019 = station_name_check_2019 %>% 
  select(to_station_name) %>% 
  unique() %>% 
  count() 
end_station_name_count_2019
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1   600

``` r
station_name_check_2020 <- select(Divvy_Trips_2020_Q1, start_station_name, end_station_name) 

start_station_name_count_2020 = station_name_check_2020 %>% 
  select(start_station_name) %>% 
  unique() %>% 
  count() 
start_station_name_count_2020
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1   607

``` r
end_station_name_count_2020 = station_name_check_2020 %>% 
  select(end_station_name) %>% 
  unique() %>% 
  count() 
end_station_name_count_2020
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1   603

## Ensure unique station names and IDs match

Station IDs and station names are related, therefore, *There must be the
same number of unique station ID’s as there are unique names* **Check
that the same for the station ID’s to ensure the count of unique values
is the same for both start and end in both years, and the count must
match the station name count above**

``` r
station_id_check_2019 <- select(Divvy_Trips_2019_Q1, from_station_id, to_station_id) 

start_station_id_count_2019 = station_id_check_2019 %>% 
  select(from_station_id) %>% 
  unique() %>% 
  count() 
start_station_id_count_2019
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1   594

``` r
end_station_id_count_2019 = station_id_check_2019 %>% 
  select(to_station_id) %>% 
  unique() %>% 
  count() 
end_station_id_count_2019
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1   600

``` r
station_id_check_2020 <- select(Divvy_Trips_2020_Q1, start_station_id, end_station_id) 

start_station_id_count_2020 = station_id_check_2020 %>% 
  select(start_station_id) %>% 
  unique() %>% 
  count() 
start_station_id_count_2020
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1   607

``` r
end_station_id_count_2020 = station_id_check_2020 %>% 
  select(end_station_id) %>% 
  unique() %>% 
  count() 
end_station_id_count_2020
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1   603

## Ensure all users are the correct user type

Users can only be subscribers or customers (2019), casual or members
(2020). *We need to ensure that users fall into one of these categories*
**filter out unique values to ensure there are only 2**

``` r
select(Divvy_Trips_2019_Q1, usertype) %>%
  unique()
```

    ## # A tibble: 2 × 1
    ##   usertype  
    ##   <chr>     
    ## 1 Subscriber
    ## 2 Customer

``` r
select(Divvy_Trips_2020_Q1, member_casual) %>%
  unique()
```

    ## # A tibble: 2 × 1
    ##   member_casual
    ##   <chr>        
    ## 1 member       
    ## 2 casual

## Ensure all bike types are the correct bike type

There are only specific kinds of bikes being used in Cyclistic,
therefore, *ensure that there are only those specific biek types in the
data set* **filter out unique bike type values in the new_2020_Q1
dataset**

``` r
unique_bike_types_2020 <- unique(Divvy_Trips_2020_Q1$rideable_type)
unique_bike_types_2020
```

    ## [1] "docked_bike"

# PHASE 3: *PROCESS*

## Delete any unnecessary customer information

In this phase I made the following considerations: (For
Divvy_Trips_2019_Q1) This is public data so you can’t identify people as
a their private info is kept hidden, therefore, *you can’t confirm if
certain casual riders.* **delete Trip_id** (For Divvy_Trips_2019_Q1)
This is public data so you can’t identify people as a their private info
is kept hidden, therefore, *you can’t confirm if certain casual riders.*
**delete ride_id** (For Divvy_Trips_2020_Q1) The stations can be
identified by name, therefore, *we don’t need exact coordinates for this
research.* **delete start_lat, end_lat, start_lng, end_lng** (For
Divvy_Trips_2020_Q1) We can’t ID customers, therefore, *we don’t need
their personal information.* **delete birthyear**

``` r
new_2019_Q1 <- select(Divvy_Trips_2019_Q1, start_time, end_time, bikeid, tripduration, from_station_id, from_station_name, to_station_id, to_station_name, usertype, gender)

new_2020_Q1 <- select(Divvy_Trips_2020_Q1, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id, member_casual)
```

## Preview the new dataframes

``` r
str(new_2019_Q1)
```

    ## tibble [365,069 × 10] (S3: tbl_df/tbl/data.frame)
    ##  $ start_time       : POSIXct[1:365069], format: "2019-01-01 00:04:37" "2019-01-01 00:08:13" ...
    ##  $ end_time         : POSIXct[1:365069], format: "2019-01-01 00:11:07" "2019-01-01 00:15:34" ...
    ##  $ bikeid           : num [1:365069] 2167 4386 1524 252 1170 ...
    ##  $ tripduration     : num [1:365069] 390 441 829 1783 364 ...
    ##  $ from_station_id  : num [1:365069] 199 44 15 123 173 98 98 211 150 268 ...
    ##  $ from_station_name: chr [1:365069] "Wabash Ave & Grand Ave" "State St & Randolph St" "Racine Ave & 18th St" "California Ave & Milwaukee Ave" ...
    ##  $ to_station_id    : num [1:365069] 84 624 644 176 35 49 49 142 148 141 ...
    ##  $ to_station_name  : chr [1:365069] "Milwaukee Ave & Grand Ave" "Dearborn St & Van Buren St (*)" "Western Ave & Fillmore St (*)" "Clark St & Elm St" ...
    ##  $ usertype         : chr [1:365069] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
    ##  $ gender           : chr [1:365069] "Male" "Female" "Female" "Male" ...

``` r
str(new_2020_Q1)
```

    ## tibble [426,887 × 8] (S3: tbl_df/tbl/data.frame)
    ##  $ rideable_type     : chr [1:426887] "docked_bike" "docked_bike" "docked_bike" "docked_bike" ...
    ##  $ started_at        : POSIXct[1:426887], format: "2020-01-21 20:06:59" "2020-01-30 14:22:39" ...
    ##  $ ended_at          : POSIXct[1:426887], format: "2020-01-21 20:14:30" "2020-01-30 14:26:22" ...
    ##  $ start_station_name: chr [1:426887] "Western Ave & Leland Ave" "Clark St & Montrose Ave" "Broadway & Belmont Ave" "Clark St & Randolph St" ...
    ##  $ start_station_id  : num [1:426887] 239 234 296 51 66 212 96 96 212 38 ...
    ##  $ end_station_name  : chr [1:426887] "Clark St & Leland Ave" "Southport Ave & Irving Park Rd" "Wilton Ave & Belmont Ave" "Fairbanks Ct & Grand Ave" ...
    ##  $ end_station_id    : num [1:426887] 326 318 117 24 212 96 212 212 96 100 ...
    ##  $ member_casual     : chr [1:426887] "member" "member" "member" "member" ...

## Start and end station columns have the same names in both dataframes

We need to ensure the column names are uniform in different tables to
make it easier to read, therefore, *start and end station columns must
both say “start_station” and “end_station”* **rename 2019 records to
start_station_name and end station_name, and rename 2020 records to
start_time, end_time, and user_type**

``` r
new_2019_Q1 <- rename(new_2019_Q1, start_station_name = from_station_name, start_station_id = from_station_id, end_station_name = to_station_name, end_station_id = to_station_id)

new_2020_Q1 <- rename(new_2020_Q1, start_time = started_at, end_time = ended_at, usertype = member_casual)
```

## User types have the same names in both dataframes

Usertypes should be consistent for easier comparison, therefore *both
2019 and 202 user types must be members and casual* **rename fields to
member and casual**

``` r
new_2019_Q1$usertype[new_2019_Q1$usertype == "Subscriber"] <- "member"

new_2019_Q1$usertype[new_2019_Q1$usertype == "Customer"] <- "casual"
```

## Check for any null values

Next we can check for any missing or null values

``` r
null_values_2019 <- new_2019_Q1[!complete.cases(new_2019_Q1),]
null_values_2019
```

    ## # A tibble: 19,711 × 10
    ##    start_time          end_time            bikeid tripduration start_station_id
    ##    <dttm>              <dttm>               <dbl>        <dbl>            <dbl>
    ##  1 2019-01-01 00:29:19 2019-01-01 01:08:12   3914         2333               35
    ##  2 2019-01-01 00:29:28 2019-01-01 01:07:49   3355         2301               35
    ##  3 2019-01-01 01:10:48 2019-01-01 01:32:28   2517         1300              290
    ##  4 2019-01-01 01:17:23 2019-01-01 01:33:30    374          967              367
    ##  5 2019-01-01 01:17:33 2019-01-01 01:33:51   1776          978              367
    ##  6 2019-01-01 01:17:59 2019-01-01 01:40:45    341         1366              316
    ##  7 2019-01-01 01:18:17 2019-01-01 01:41:01   4507         1364              316
    ##  8 2019-01-01 01:23:13 2019-01-01 02:07:47    628         2674              260
    ##  9 2019-01-01 01:46:09 2019-01-01 02:07:32   4333         1283               35
    ## 10 2019-01-01 01:46:14 2019-01-01 02:07:13   3077         1259               35
    ## # ℹ 19,701 more rows
    ## # ℹ 5 more variables: start_station_name <chr>, end_station_id <dbl>,
    ## #   end_station_name <chr>, usertype <chr>, gender <chr>

## Delete irrelevant rideable_type

We realised that the rideable_type column in the new_2020_Q1 does not
refer to bike_types **delete rideable_type column**

``` r
new_2020_Q1 <-select(new_2020_Q1, !rideable_type)
```

## Convert tripduration to the correct data type

Trip duration is a time-based field **We need to change tripduration in
2019 to be a time(secs)**

``` r
new_2019_Q1$tripduration <- as.duration(new_2019_Q1$tripduration)
```

## Calculate the tripdurations for 2020

We need to calculate the tripdurations for 2020 data **use difftime from
start to end date**

``` r
new_2020_Q1 <- mutate(new_2020_Q1, tripduration = as.duration(difftime(new_2020_Q1$end_time, new_2020_Q1$start_time, , units = "secs")))
```

## Calcuate start weekday from start_time

We need to see what weekday each bike was rented in both tables **add
new column with weekday to data**

``` r
new_2019_Q1 <- mutate(new_2019_Q1, start_weekday = weekdays(new_2019_Q1$start_time))

new_2020_Q1 <- mutate(new_2020_Q1, start_weekday = weekdays(new_2020_Q1$start_time))
```

## Merge dataframes together for analysis

We need to be able to join the 2 tables together for the most fulfilling
analysis, therefore, *we need to create new data frames in the correct
order*

**2019:** We don’t need the biked; **2019:** We do need the gender;
**2020:** Add a new column for gender so that it lines up with 2019
table

There is no primary key but each of the new dataframes have the same
columns/vectors

``` r
join_2019_Q1_data <- select(new_2019_Q1, usertype, start_time, start_weekday, tripduration, start_station_id, start_station_name, end_station_id, end_station_name, gender)
join_2019_Q1_data
```

    ## # A tibble: 365,069 × 9
    ##    usertype start_time          start_weekday tripduration          
    ##    <chr>    <dttm>              <chr>         <Duration>            
    ##  1 member   2019-01-01 00:04:37 Tuesday       390s (~6.5 minutes)   
    ##  2 member   2019-01-01 00:08:13 Tuesday       441s (~7.35 minutes)  
    ##  3 member   2019-01-01 00:13:23 Tuesday       829s (~13.82 minutes) 
    ##  4 member   2019-01-01 00:13:45 Tuesday       1783s (~29.72 minutes)
    ##  5 member   2019-01-01 00:14:52 Tuesday       364s (~6.07 minutes)  
    ##  6 member   2019-01-01 00:15:33 Tuesday       216s (~3.6 minutes)   
    ##  7 member   2019-01-01 00:16:06 Tuesday       177s (~2.95 minutes)  
    ##  8 member   2019-01-01 00:18:41 Tuesday       100s (~1.67 minutes)  
    ##  9 member   2019-01-01 00:18:43 Tuesday       1727s (~28.78 minutes)
    ## 10 member   2019-01-01 00:19:18 Tuesday       336s (~5.6 minutes)   
    ## # ℹ 365,059 more rows
    ## # ℹ 5 more variables: start_station_id <dbl>, start_station_name <chr>,
    ## #   end_station_id <dbl>, end_station_name <chr>, gender <chr>

``` r
join_2020_Q1_data <- new_2020_Q1 %>%
  mutate(gender = NA) %>%
  select(usertype, start_time, start_weekday, tripduration, start_station_id, start_station_name, end_station_id, end_station_name, gender)
join_2020_Q1_data
```

    ## # A tibble: 426,887 × 9
    ##    usertype start_time          start_weekday tripduration        
    ##    <chr>    <dttm>              <chr>         <Duration>          
    ##  1 member   2020-01-21 20:06:59 Tuesday       451s (~7.52 minutes)
    ##  2 member   2020-01-30 14:22:39 Thursday      223s (~3.72 minutes)
    ##  3 member   2020-01-09 19:29:26 Thursday      171s (~2.85 minutes)
    ##  4 member   2020-01-06 16:17:07 Monday        529s (~8.82 minutes)
    ##  5 member   2020-01-30 08:37:16 Thursday      332s (~5.53 minutes)
    ##  6 member   2020-01-10 12:33:05 Friday        289s (~4.82 minutes)
    ##  7 member   2020-01-10 13:07:35 Friday        289s (~4.82 minutes)
    ##  8 member   2020-01-10 07:24:53 Friday        297s (~4.95 minutes)
    ##  9 member   2020-01-31 16:37:16 Friday        295s (~4.92 minutes)
    ## 10 member   2020-01-31 09:39:17 Friday        203s (~3.38 minutes)
    ## # ℹ 426,877 more rows
    ## # ℹ 5 more variables: start_station_id <dbl>, start_station_name <chr>,
    ## #   end_station_id <dbl>, end_station_name <chr>, gender <lgl>

``` r
final_data_2019_2020 <- rbind(join_2019_Q1_data, join_2020_Q1_data)
final_data_2019_2020
```

    ## # A tibble: 791,956 × 9
    ##    usertype start_time          start_weekday tripduration          
    ##    <chr>    <dttm>              <chr>         <Duration>            
    ##  1 member   2019-01-01 00:04:37 Tuesday       390s (~6.5 minutes)   
    ##  2 member   2019-01-01 00:08:13 Tuesday       441s (~7.35 minutes)  
    ##  3 member   2019-01-01 00:13:23 Tuesday       829s (~13.82 minutes) 
    ##  4 member   2019-01-01 00:13:45 Tuesday       1783s (~29.72 minutes)
    ##  5 member   2019-01-01 00:14:52 Tuesday       364s (~6.07 minutes)  
    ##  6 member   2019-01-01 00:15:33 Tuesday       216s (~3.6 minutes)   
    ##  7 member   2019-01-01 00:16:06 Tuesday       177s (~2.95 minutes)  
    ##  8 member   2019-01-01 00:18:41 Tuesday       100s (~1.67 minutes)  
    ##  9 member   2019-01-01 00:18:43 Tuesday       1727s (~28.78 minutes)
    ## 10 member   2019-01-01 00:19:18 Tuesday       336s (~5.6 minutes)   
    ## # ℹ 791,946 more rows
    ## # ℹ 5 more variables: start_station_id <dbl>, start_station_name <chr>,
    ## #   end_station_id <dbl>, end_station_name <chr>, gender <chr>

## Write the final dataframe to .csv file for visualisation

Now we export the final dataset for analysis and visualisation in
Tableau

``` r
write.csv(final_data_2019_2020, "final_data_2019_2020.csv")
```

# PHASE 4: *ANALYSE*

## Tableau Data Viz

Prepare Visualisations in Tableau

<div id="viz1745408112450" class="tableauPlaceholder"
style="position: relative">

<noscript>
<a href='#'><img alt='High-Level Understanding Of The Difference Between Casual Customers And Members At Cyclistic  ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ta&#47;TaskADataViz&#47;Story1&#47;1_rss.png' style='border: none' /></a>
</noscript>
<object class="tableauViz" style="display:none;">
<param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' />
<param name='embed_code_version' value='3' />
<param name='site_root' value='' /><param name='name' value='TaskADataViz&#47;Story1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ta&#47;TaskADataViz&#47;Story1&#47;1.png' />
<param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /><param name='filter' value='publish=yes' />
</object>

</div>

``` js

 var divElement = document.getElementById('viz1745408112450');                    
 var vizElement = divElement.getElementsByTagName('object')[0];                    
 vizElement.style.width='1169px';vizElement.style.height='854px';                    
 var scriptElement = document.createElement('script');                    
 scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    
 vizElement.parentNode.insertBefore(scriptElement, vizElement);
```

<script>
&#10; var divElement = document.getElementById('viz1745408112450');                    
 var vizElement = divElement.getElementsByTagName('object')[0];                    
 vizElement.style.width='1169px';vizElement.style.height='854px';                    
 var scriptElement = document.createElement('script');                    
 scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    
 vizElement.parentNode.insertBefore(scriptElement, vizElement);
&#10;</script>
# PHASE 5: *SHARE*

Present clear findings and insights from each of the data visualisations

### 1. Provide an overview of how customers are split into categories

#### Casual Vs. Members Data Viz

**Finding:** Over 90% of Cyclistic’s trips are by members (71743).
**Insight:** *71743 is not a negligible number of bike trips, and thus
confirms the belief that there is an opportunity to increase profits by
developing marketing campaigns and initiatives to convert casual riders
to members.*

#### Male Member vs Female Member Trips Data Viz

**Finding:** We can see that the majority of Cyclistic customers are
male, however, what’s interesting is that females make up 31.59% of
casual riders (in comparison to the 19.16% of female members).
**Insight:** *This means that future marketing campaigns and initiatives
should also be targeted at converting female casual riders to members.*

#### Average Trip Duration Oer User Type (Minutes) Data Viz

**Finding:** We can see that the average duration for casual trip is
significantly higher than for member trips. **Insight:** *We need to
investigate why and perhaps tailor marketing campaigns and initiatives
to take this difference into consideration by making it beneficial for
riders with long trips to become members*

### 2. Examine total trip count patterns across hours/weekdays for each user type

#### Number of Member Trips Per Weekday

**Finding:** Casual Member Trips are at their highest on weekends
(Saturday - Sunday), whereas member trips are at their highest during
the weekday (Monday - Friday) **Insight:** *Marketing campaigns and
initiatives should make becoming a member beneficial for those taking
bike trips on weekends (Saturday-Sunday)*

#### Volume of Customers Starting Rides Throughout The Day

**Finding:** The volume of casual member trips being started through out
the day steadily rises from 6am until its peak at 5pm, whereas the
volume of member bike trips being started sees a sharp spike at 8am and
5pm (most likely because member are at work). Member trips are very low
between 10:00 and 14:00 **Insight:** *Marketing campaigns and
initiatives should make it beneficial for members that start trips
between 10:00 and 14:00 inn order to attract the casual riders during
those hours to convert to membership*

### 3. Examine main bike trip starting and ending points for each user type

**Finding:** The most popular starting and ending stations for casual
trips are completely different to those for member trips **Insight:**
*Marketing campaigns and initiatives should make it beneficial for
members to start/end their trips at stations that are popular for casual
trips as to drive conversion to membership at these casual trip
hotspots.*
