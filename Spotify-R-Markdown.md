Spotify R Markdown
================
Shima Mojapelo
2025-04-28

# **PHASE 1: ASK**

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
    ##  C:\Users\shima\AppData\Local\Temp\Rtmpo3WcXX\downloaded_packages

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
    ##  C:\Users\shima\AppData\Local\Temp\Rtmpo3WcXX\downloaded_packages

``` r
install.packages("ggplot2")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'ggplot2' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\Rtmpo3WcXX\downloaded_packages

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
    ##  C:\Users\shima\AppData\Local\Temp\Rtmpo3WcXX\downloaded_packages

``` r
install.packages("sqldf")
```

    ## Installing package into 'C:/Users/shima/AppData/Local/R/win-library/4.5'
    ## (as 'lib' is unspecified)

    ## package 'sqldf' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\shima\AppData\Local\Temp\Rtmpo3WcXX\downloaded_packages

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

## Load Spotify datasets into dataframes

``` r
high_popularity_data <- read_csv("Spotify Data/high_popularity_spotify_data.csv") 
```

    ## Rows: 1686 Columns: 29
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (15): playlist_genre, track_artist, track_href, uri, track_album_name, p...
    ## dbl (14): energy, tempo, danceability, loudness, liveness, valence, time_sig...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(high_popularity_data)
```

    ## # A tibble: 6 × 29
    ##   energy tempo danceability playlist_genre loudness liveness valence
    ##    <dbl> <dbl>        <dbl> <chr>             <dbl>    <dbl>   <dbl>
    ## 1  0.592  158.        0.521 pop               -7.78   0.122    0.535
    ## 2  0.507  105.        0.747 pop              -10.2    0.117    0.438
    ## 3  0.808  109.        0.554 pop               -4.17   0.159    0.372
    ## 4  0.91   113.        0.67  pop               -4.07   0.304    0.786
    ## 5  0.783  149.        0.777 pop               -4.48   0.355    0.939
    ## 6  0.582  117.        0.7   pop               -5.96   0.0881   0.785
    ## # ℹ 22 more variables: track_artist <chr>, time_signature <dbl>,
    ## #   speechiness <dbl>, track_popularity <dbl>, track_href <chr>, uri <chr>,
    ## #   track_album_name <chr>, playlist_name <chr>, analysis_url <chr>,
    ## #   track_id <chr>, track_name <chr>, track_album_release_date <chr>,
    ## #   instrumentalness <dbl>, track_album_id <chr>, mode <dbl>, key <dbl>,
    ## #   duration_ms <dbl>, acousticness <dbl>, id <chr>, playlist_subgenre <chr>,
    ## #   type <chr>, playlist_id <chr>

``` r
low_popularity_data <- read_csv("Spotify Data/low_popularity_spotify_data.csv")
```

    ## Rows: 3145 Columns: 29
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (15): playlist_name, track_artist, playlist_genre, playlist_subgenre, tr...
    ## dbl (14): time_signature, track_popularity, speechiness, danceability, durat...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
head(low_popularity_data)
```

    ## # A tibble: 6 × 29
    ##   time_signature track_popularity speechiness danceability playlist_name
    ##            <dbl>            <dbl>       <dbl>        <dbl> <chr>        
    ## 1              4               23      0.0393        0.636 Rock Classics
    ## 2              4               53      0.0317        0.572 Rock Classics
    ## 3              4               55      0.0454        0.591 Rock Classics
    ## 4              4               64      0.101         0.443 Jazz Classics
    ## 5              4               62      0.0298        0.685 Jazz Classics
    ## 6              4               61      0.0566        0.67  Jazz Classics
    ## # ℹ 24 more variables: track_artist <chr>, duration_ms <dbl>, energy <dbl>,
    ## #   playlist_genre <chr>, playlist_subgenre <chr>, track_href <chr>,
    ## #   track_name <chr>, mode <dbl>, uri <chr>, type <chr>,
    ## #   track_album_release_date <chr>, analysis_url <chr>, id <chr>,
    ## #   instrumentalness <dbl>, track_album_id <chr>, playlist_id <chr>,
    ## #   track_id <chr>, valence <dbl>, key <dbl>, tempo <dbl>, loudness <dbl>,
    ## #   acousticness <dbl>, liveness <dbl>, track_album_name <chr>

# **PHASE 3: PROCESSES**

## Assess the data integrity

On investigation the data sets seem to have been thoroughly cleaned
before making it open to the public.

## Combine tables for analysis

``` r
popularity_data <- rbind(high_popularity_data, low_popularity_data)
head(popularity_data)
```

    ## # A tibble: 6 × 29
    ##   energy tempo danceability playlist_genre loudness liveness valence
    ##    <dbl> <dbl>        <dbl> <chr>             <dbl>    <dbl>   <dbl>
    ## 1  0.592  158.        0.521 pop               -7.78   0.122    0.535
    ## 2  0.507  105.        0.747 pop              -10.2    0.117    0.438
    ## 3  0.808  109.        0.554 pop               -4.17   0.159    0.372
    ## 4  0.91   113.        0.67  pop               -4.07   0.304    0.786
    ## 5  0.783  149.        0.777 pop               -4.48   0.355    0.939
    ## 6  0.582  117.        0.7   pop               -5.96   0.0881   0.785
    ## # ℹ 22 more variables: track_artist <chr>, time_signature <dbl>,
    ## #   speechiness <dbl>, track_popularity <dbl>, track_href <chr>, uri <chr>,
    ## #   track_album_name <chr>, playlist_name <chr>, analysis_url <chr>,
    ## #   track_id <chr>, track_name <chr>, track_album_release_date <chr>,
    ## #   instrumentalness <dbl>, track_album_id <chr>, mode <dbl>, key <dbl>,
    ## #   duration_ms <dbl>, acousticness <dbl>, id <chr>, playlist_subgenre <chr>,
    ## #   type <chr>, playlist_id <chr>

## Export new combined data

``` r
write.csv(popularity_data, "Spotify Data/Combined_Data.csv")
```

# **PHASE 4: ANALYSE**

## Visualise Data

<div id="viz1745854158614" class="tableauPlaceholder"
style="position: relative">

<noscript>
<a href='#'><img alt='Spotify Data Analysis ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Sp&#47;SpotifyDataAnalysis_17458531912460&#47;Story1&#47;1_rss.png' style='border: none' /></a>
</noscript>
<object class="tableauViz" style="display:none;">
<param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' />
<param name='embed_code_version' value='3' />
<param name='site_root' value='' /><param name='name' value='SpotifyDataAnalysis_17458531912460&#47;Story1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Sp&#47;SpotifyDataAnalysis_17458531912460&#47;Story1&#47;1.png' />
<param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /><param name='filter' value='publish=yes' />
</object>

</div>

``` js

var divElement = document.getElementById('viz1745854158614');                    
ar vizElement = divElement.getElementsByTagName('object')[0];                    
vizElement.style.width='1169px';vizElement.style.height='854px';                    
var scriptElement = document.createElement('script');                    
scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    
vizElement.parentNode.insertBefore(scriptElement, vizElement);
```

<script>
&#10;var divElement = document.getElementById('viz1745854158614');                    
ar vizElement = divElement.getElementsByTagName('object')[0];                    
vizElement.style.width='1169px';vizElement.style.height='854px';                    
var scriptElement = document.createElement('script');                    
scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    
vizElement.parentNode.insertBefore(scriptElement, vizElement);
&#10;</script>

# **PHASE 5: SHARE INSIGHTS**

**Findings:** R&B, gaming and Metal seem to be the most popular genres
**Recommendations:** Promote more songs in these genres

**Findings:** Sabrina Carpenter, Bruno Mars, Billie Eilish, Kendrick
Lamar and Linkin Park are the most popular artists on Spotify
**Recommendations:** Promote more songs from these artists

**Findings:** Songs with tempos between 155- 160 and 200 - 205 seem to
be the most popular tracks **Recommendations:** Promote more songs that
fall inot these genres

**Findings:** The more danceable a song is the more popular it is
**Recommendations:** Promote more songs with a high danceability score

**Findings:** Songs with speechinesss score of between 0.6 - 0.7 or
0.8 - 0.9 seem to be the most popular **Recommendations:** Promote sings
within these speechiness levels more

**Findings:** Songs with a loudness level between -5 and +5 dB are the
most popular **Recommendations:** Promote songs with higher loudness
levels
