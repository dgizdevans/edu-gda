How Does a Bike-Share Navigate Speedy
================
Dmitrijs Gizdevans
1/20/2022

## Intro

This Case study is the Capstone project of the Google Data Analytics
Professional Certificate courses. This project shows how data analysis
concepts and techniques can be used to solve a business problem from a
fictitious company.

More information about the project’s input data can be viewed at the
<a href="https://d3c33hcgiwev3.cloudfront.net/aacF81H_TsWnBfNR_x7FIg_36299b28fa0c4a5aba836111daad12f1_DAC8-Case-Study-1.pdf?Expires=1631404800&Signature=KHTVUCv2tp8pzmhKyh1zPHnEe2WeNWU8dvAqWs1WAEvdV0of18RQDDdx7MMWpJeI-xozOsL7qcTnjcdLOuSEDkz3blC603egHP4PueO-Buoj0Ps-6sdr5hHuTbKKAZk2yN~64xiA3djdCkcajU4BRO8ttj9AMq8qJtbfmAkIepI_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A">
link</a>

<b>Project scenario:</b>

You are a junior data analyst working in the marketing analyst team at
Cyclistic, a bike-share company in Chicago. The director of marketing
believes the company’s future success depends on maximizing the number
of annual memberships. Therefore, your team wants to understand how
casual riders and annual members use Cyclistic bikes differently. From
these insights, your team will design a new marketing strategy to
convert casual riders into annual members. But first, Cyclistic
executives must approve your recommendations, so they must be backed up
with compelling data insights and professional data visualizations.

# Project: How Does a Bike-Share Navigate Speedy

## Project overview and key questions

<b>Topic:</b> Characteristics of the use by users of the bike-sharing
service Cyclists in Chicago.

<b>Problem:</b> Increasing riders’ conversions to annual membership from
casual bicycle users in the bike-share company Cyclists in Chicago.

<b>Stakeholders:</b> Director of Cyclistic executive team.

<b>Deliverable:</b> A detailed analysis of the characteristics and
activities of various types of service users over the past 12 months
including: the clear statement of business tasks, data sources, data
cleaning, and manipulations overview, summarizes analyses,
visualizations, and at least three recommendations based on this
analysis.

<b>The analysis should answer three main questions:</b></br></br> *1.
How do annual members and casual riders use Cyclistic bikes
differently?*</br> *2. Why would casual riders buy Cyclistic annual
memberships?*</br> *3. How can Cyclistic use digital media to influence
casual riders to become members?*</br>

## Data preparation

<b>Data description:</b> The company has its own data set for period
from April 2020 till December 2021. Since this is the data of the first
person, which was collected and managed with the consent of all users of
the service under the guidance of the company’s specialists. The level
of trust in the data is extremely high.

The data is collected and stored by an external contractor, Motivate
International Inc., and processed under
<a href="https://ride.divvybikes.com/data-license-agreement">license</a>.

<b>Data location:</b> AWS S3
Storage:<https://divvy-tripdata.s3.amazonaws.com/index.html>

<b>Data organization:</b> The data is stored in csv files, each month
separately.

### Initial data evaluation and data download

The files are hosted on a third-party service in archives, so the
initial task is to download them and check for integrity. Since it will
be a long time task using R, I use preprocessing and downloading just in
bash, and after that, we can more easily do everything in R and
notebook.

I take all the links to the archives and write them to a shared file
list.txt

![](img1.png)

Then I run the wget utility, which downloads all the necessary files
from the list.

$ wget -vi list.txt -P raw-data -o log.log

After a while we get these files on the local computer.

$ ls

![](img2.png)

Unpack all the archives and move the csv files we need to one, separate
directory.

$ unzip “\*.zip”

$ mv \*.csv data

![](img3.png)

As we can see, we have 21 files for 21 months, but since according to
the analysis conditions we need only 12 previous months, we select files
from January to December 2021. In order for us to upload them to a
laptop (both Kaggle and Jupiter), we will place CSV files on the my
server.

### Uploading files to notebook (work environment)

After we have collected the raw data, selected the necessary data and
consolidated them in one place, we need to upload them to the analyst’s
working environment (Jupyter notebook, Kaggle and Rstudio), in my case
this is Jupyter.</br>

``` r
# Specify place (URL) where data files is stored / Start from this point if all .csv files DON'T uploaded locally  

url01 <-"https://lab73.digital/202101-divvy-tripdata.csv"
url02 <-"https://lab73.digital/202102-divvy-tripdata.csv"
url03 <-"https://lab73.digital/202103-divvy-tripdata.csv"
url04 <-"https://lab73.digital/202104-divvy-tripdata.csv"
url05 <-"https://lab73.digital/202105-divvy-tripdata.csv"
url06 <-"https://lab73.digital/202106-divvy-tripdata.csv"
url07 <-"https://lab73.digital/202107-divvy-tripdata.csv"
url08 <-"https://lab73.digital/202108-divvy-tripdata.csv"
url09 <-"https://lab73.digital/202109-divvy-tripdata.csv"
url10 <-"https://lab73.digital/202110-divvy-tripdata.csv"
url11 <-"https://lab73.digital/202111-divvy-tripdata.csv"
url12 <-"https://lab73.digital/202112-divvy-tripdata.csv"
```

``` r
# Specify destination where data file should be saved and files names
dest01 <- "data202101.csv"
dest02 <- "data202102.csv"
dest03 <- "data202103.csv"
dest04 <- "data202104.csv"
dest05 <- "data202105.csv"
dest06 <- "data202106.csv"
dest07 <- "data202107.csv"
dest08 <- "data202108.csv"
dest09 <- "data202109.csv"
dest10 <- "data202110.csv"
dest11 <- "data202111.csv"
dest12 <- "data202112.csv"
```

``` r
# Downloading files to local environment // VERY HEAVY TASK // if not exist in local env 
if (file.exists("data202101.csv")) {
  print("file exist")
} else {
  download.file(url01, dest01)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202102.csv")) {
  print("file exist")
} else {
  download.file(url02, dest02)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202103.csv")) {
  print("file exist")
} else {
  download.file(url03, dest03)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202104.csv")) {
  print("file exist")
} else {
  download.file(url04, dest04)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202105.csv")) {
  print("file exist")
} else {
  download.file(url05, dest05)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202106.csv")) {
  print("file exist")
} else {
  download.file(url06, dest06)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202107.csv")) {
  print("file exist")
} else {
  download.file(url07, dest07)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202108.csv")) {
  print("file exist")
} else {
  download.file(url08, dest08)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202109.csv")) {
  print("file exist")
} else {
  download.file(url09, dest09)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202110.csv")) {
  print("file exist")
} else {
  download.file(url10, dest10)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202111.csv")) {
  print("file exist")
} else {
  download.file(url11, dest11)
}
```

    ## [1] "file exist"

``` r
if (file.exists("data202111.csv")) {
  print("file exist")
} else {
  download.file(url12, dest12)
}
```

    ## [1] "file exist"

After downloading, we need to check if all the files have been
downloaded and are available in the local environment

``` r
# Checking if all files exists after previous step 

file.exists("data202101.csv") &
  file.exists("data202102.csv") &
  file.exists("data202103.csv") &
  file.exists("data202104.csv") &
  file.exists("data202105.csv") &
  file.exists("data202106.csv") &
  file.exists("data202107.csv") &
  file.exists("data202108.csv") &
  file.exists("data202109.csv") &
  file.exists("data202110.csv") &
  file.exists("data202111.csv") & 
  file.exists("data202112.csv")
```

    ## [1] TRUE

If the answer is True, it mean all data is dowloaded and the first stage
of data preprocessing went well, all files were uploaded to the local
environment, and you can proceed to import data from .csv into a
dataframe for further work with them.

``` r
# Converting .csv to dataframes

df_202101 <- read.csv("data202101.csv")
df_202102 <- read.csv("data202102.csv")
df_202103 <- read.csv("data202103.csv")
df_202104 <- read.csv("data202104.csv")
df_202105 <- read.csv("data202105.csv")
df_202106 <- read.csv("data202106.csv")
df_202107 <- read.csv("data202107.csv")
df_202108 <- read.csv("data202108.csv")
df_202109 <- read.csv("data202109.csv")
df_202110 <- read.csv("data202110.csv")
df_202111 <- read.csv("data202111.csv")
df_202112 <- read.csv("data202112.csv")
```

At the stage of data preparation, our task was the initial preparation
of data for processing. The data has been downloaded and prepared in the
form of dataframes. Let’s start processing.

## Data processing

At the second stage (Process), we must fully prepare the data for
analysis. Check the structure and integrity of data, compare the
uniformity of data types for all dataframes, collect data for
convenience in one dataframe, remove redundant data.

### Preparing the environment for data processing and data analysis

Getting started with data processing, we will upload all the necessary
packages to the environment.

``` r
# Installing packages 
install.packages("tidyverse", repos = "https://cran.rstudio.com")
```

    ## Installing package into 'C:/Users/gizhd/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## package 'tidyverse' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\gizhd\AppData\Local\Temp\RtmpQlPGdL\downloaded_packages

``` r
install.packages("tidyr", repos = "https://cran.rstudio.com")
```

    ## Installing package into 'C:/Users/gizhd/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## package 'tidyr' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\gizhd\AppData\Local\Temp\RtmpQlPGdL\downloaded_packages

``` r
install.packages("dplyr", repos = "https://cran.rstudio.com")
```

    ## Installing package into 'C:/Users/gizhd/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## package 'dplyr' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\gizhd\AppData\Local\Temp\RtmpQlPGdL\downloaded_packages

``` r
install.packages("lubridate", repos = "https://cran.rstudio.com")
```

    ## Installing package into 'C:/Users/gizhd/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## package 'lubridate' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\gizhd\AppData\Local\Temp\RtmpQlPGdL\downloaded_packages

``` r
install.packages("readr", repos = "https://cran.rstudio.com")
```

    ## Installing package into 'C:/Users/gizhd/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## package 'readr' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\gizhd\AppData\Local\Temp\RtmpQlPGdL\downloaded_packages

``` r
install.packages("ggplot2", repos = "https://cran.rstudio.com")
```

    ## Installing package into 'C:/Users/gizhd/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## package 'ggplot2' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\gizhd\AppData\Local\Temp\RtmpQlPGdL\downloaded_packages

``` r
install.packages("janitor", repos = "https://cran.rstudio.com")
```

    ## Installing package into 'C:/Users/gizhd/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## package 'janitor' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\gizhd\AppData\Local\Temp\RtmpQlPGdL\downloaded_packages

``` r
install.packages("data.table", repos = "https://cran.rstudio.com")
```

    ## Installing package into 'C:/Users/gizhd/OneDrive/Documents/R/win-library/4.1'
    ## (as 'lib' is unspecified)

    ## package 'data.table' successfully unpacked and MD5 sums checked
    ## 
    ## The downloaded binary packages are in
    ##  C:\Users\gizhd\AppData\Local\Temp\RtmpQlPGdL\downloaded_packages

``` r
# Loading libraries 
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.6     v dplyr   1.0.7
    ## v tidyr   1.1.4     v stringr 1.4.0
    ## v readr   2.1.1     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(tidyr)
library(dplyr)
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(readr)
library(ggplot2)
library(janitor)
```

    ## 
    ## Attaching package: 'janitor'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     chisq.test, fisher.test

``` r
library(data.table)
```

    ## 
    ## Attaching package: 'data.table'

    ## The following objects are masked from 'package:lubridate':
    ## 
    ##     hour, isoweek, mday, minute, month, quarter, second, wday, week,
    ##     yday, year

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     between, first, last

    ## The following object is masked from 'package:purrr':
    ## 
    ##     transpose

### Data structure analysis

Despite the fact that the data is collected by specialists, we will
still check whether the data types and column names match in all
dataframes.

``` r
# Comparison of data types in dataframes
compare_df_cols(df_202101, 
           df_202102,
           df_202103,
           df_202104, 
           df_202105,
           df_202106,
           df_202107, 
           df_202108,
           df_202109,
           df_202110, 
           df_202111,
           df_202112)
```

    ##           column_name df_202101 df_202102 df_202103 df_202104 df_202105
    ## 1             end_lat   numeric   numeric   numeric   numeric   numeric
    ## 2             end_lng   numeric   numeric   numeric   numeric   numeric
    ## 3      end_station_id character character character character character
    ## 4    end_station_name character character character character character
    ## 5            ended_at character character character character character
    ## 6       member_casual character character character character character
    ## 7             ride_id character character character character character
    ## 8       rideable_type character character character character character
    ## 9           start_lat   numeric   numeric   numeric   numeric   numeric
    ## 10          start_lng   numeric   numeric   numeric   numeric   numeric
    ## 11   start_station_id character character character character character
    ## 12 start_station_name character character character character character
    ## 13         started_at character character character character character
    ##    df_202106 df_202107 df_202108 df_202109 df_202110 df_202111 df_202112
    ## 1    numeric   numeric   numeric   numeric   numeric   numeric   numeric
    ## 2    numeric   numeric   numeric   numeric   numeric   numeric   numeric
    ## 3  character character character character character character character
    ## 4  character character character character character character character
    ## 5  character character character character character character character
    ## 6  character character character character character character character
    ## 7  character character character character character character character
    ## 8  character character character character character character character
    ## 9    numeric   numeric   numeric   numeric   numeric   numeric   numeric
    ## 10   numeric   numeric   numeric   numeric   numeric   numeric   numeric
    ## 11 character character character character character character character
    ## 12 character character character character character character character
    ## 13 character character character character character character character

The results of the comparison clearly show that the data in all
dataframes are homogeneous.

### Data merging

From the previous analysis of the received data frames, it can be
concluded that the data has a high level of integrity and can be
combined into a single data frame for further analysis of the data
itself, and not their structure.

We have the same data type and number of columns in all dataframes, then
we can merge them simply with the rbind function.

``` r
# Mergnig all dataframes to one 
df_2021 <- rbind(df_202101, 
               df_202102,
               df_202103,
               df_202104, 
               df_202105,
               df_202106,
               df_202107, 
               df_202108,
               df_202109,
               df_202110, 
               df_202111,
               df_202112)  
```

Let’s check the first and last five entries in the new dataframe and
compare them with similar ones in the original dataframe.

``` r
# Compare first five rows in original and new dataframes 
head(df_2021, n = 5)
```

    ##            ride_id rideable_type          started_at            ended_at
    ## 1 E19E6F1B8D4C42ED electric_bike 2021-01-23 16:14:19 2021-01-23 16:24:44
    ## 2 DC88F20C2C55F27F electric_bike 2021-01-27 18:43:08 2021-01-27 18:47:12
    ## 3 EC45C94683FE3F27 electric_bike 2021-01-21 22:35:54 2021-01-21 22:37:14
    ## 4 4FA453A75AE377DB electric_bike 2021-01-07 13:31:13 2021-01-07 13:42:55
    ## 5 BE5E8EB4E7263A0B electric_bike 2021-01-23 02:24:02 2021-01-23 02:24:45
    ##           start_station_name start_station_id end_station_name end_station_id
    ## 1 California Ave & Cortez St            17660                                
    ## 2 California Ave & Cortez St            17660                                
    ## 3 California Ave & Cortez St            17660                                
    ## 4 California Ave & Cortez St            17660                                
    ## 5 California Ave & Cortez St            17660                                
    ##   start_lat start_lng end_lat end_lng member_casual
    ## 1  41.90034 -87.69674   41.89  -87.72        member
    ## 2  41.90033 -87.69671   41.90  -87.69        member
    ## 3  41.90031 -87.69664   41.90  -87.70        member
    ## 4  41.90040 -87.69666   41.92  -87.69        member
    ## 5  41.90033 -87.69670   41.90  -87.70        casual

``` r
head(df_202101, n = 5)
```

    ##            ride_id rideable_type          started_at            ended_at
    ## 1 E19E6F1B8D4C42ED electric_bike 2021-01-23 16:14:19 2021-01-23 16:24:44
    ## 2 DC88F20C2C55F27F electric_bike 2021-01-27 18:43:08 2021-01-27 18:47:12
    ## 3 EC45C94683FE3F27 electric_bike 2021-01-21 22:35:54 2021-01-21 22:37:14
    ## 4 4FA453A75AE377DB electric_bike 2021-01-07 13:31:13 2021-01-07 13:42:55
    ## 5 BE5E8EB4E7263A0B electric_bike 2021-01-23 02:24:02 2021-01-23 02:24:45
    ##           start_station_name start_station_id end_station_name end_station_id
    ## 1 California Ave & Cortez St            17660                                
    ## 2 California Ave & Cortez St            17660                                
    ## 3 California Ave & Cortez St            17660                                
    ## 4 California Ave & Cortez St            17660                                
    ## 5 California Ave & Cortez St            17660                                
    ##   start_lat start_lng end_lat end_lng member_casual
    ## 1  41.90034 -87.69674   41.89  -87.72        member
    ## 2  41.90033 -87.69671   41.90  -87.69        member
    ## 3  41.90031 -87.69664   41.90  -87.70        member
    ## 4  41.90040 -87.69666   41.92  -87.69        member
    ## 5  41.90033 -87.69670   41.90  -87.70        casual

``` r
# Compare last  five rows in original and new dataframes 
tail(df_2021, n = 5)
```

    ##                  ride_id rideable_type          started_at            ended_at
    ## 5595059 847431F3D5353AB7 electric_bike 2021-12-12 13:36:55 2021-12-12 13:56:08
    ## 5595060 CF407BBC3B9FAD63 electric_bike 2021-12-06 19:37:50 2021-12-06 19:44:51
    ## 5595061 60BB69EBF5440E92 electric_bike 2021-12-02 08:57:04 2021-12-02 09:05:21
    ## 5595062 C414F654A28635B8 electric_bike 2021-12-13 09:00:26 2021-12-13 09:14:39
    ## 5595063 37AC57E34B2E7E97  classic_bike 2021-12-13 08:45:32 2021-12-13 08:49:09
    ##                  start_station_name start_station_id         end_station_name
    ## 5595059       Canal St & Madison St            13341                         
    ## 5595060       Canal St & Madison St            13341 Kingsbury St & Kinzie St
    ## 5595061       Canal St & Madison St            13341  Dearborn St & Monroe St
    ## 5595062      Lawndale Ave & 16th St            362.0                         
    ## 5595063 Michigan Ave & Jackson Blvd     TA1309000002  Dearborn St & Monroe St
    ##         end_station_id start_lat start_lng  end_lat   end_lng member_casual
    ## 5595059                 41.88229 -87.63975 41.89000 -87.61000        casual
    ## 5595060   KA1503000043  41.88212 -87.64005 41.88911 -87.63886        member
    ## 5595061   TA1305000006  41.88196 -87.63995 41.88025 -87.62960        member
    ## 5595062                 41.86000 -87.72000 41.85000 -87.71000        member
    ## 5595063   TA1305000006  41.87785 -87.62408 41.88132 -87.62952        member

``` r
tail(df_202112, n = 5)
```

    ##                 ride_id rideable_type          started_at            ended_at
    ## 247536 847431F3D5353AB7 electric_bike 2021-12-12 13:36:55 2021-12-12 13:56:08
    ## 247537 CF407BBC3B9FAD63 electric_bike 2021-12-06 19:37:50 2021-12-06 19:44:51
    ## 247538 60BB69EBF5440E92 electric_bike 2021-12-02 08:57:04 2021-12-02 09:05:21
    ## 247539 C414F654A28635B8 electric_bike 2021-12-13 09:00:26 2021-12-13 09:14:39
    ## 247540 37AC57E34B2E7E97  classic_bike 2021-12-13 08:45:32 2021-12-13 08:49:09
    ##                 start_station_name start_station_id         end_station_name
    ## 247536       Canal St & Madison St            13341                         
    ## 247537       Canal St & Madison St            13341 Kingsbury St & Kinzie St
    ## 247538       Canal St & Madison St            13341  Dearborn St & Monroe St
    ## 247539      Lawndale Ave & 16th St            362.0                         
    ## 247540 Michigan Ave & Jackson Blvd     TA1309000002  Dearborn St & Monroe St
    ##        end_station_id start_lat start_lng  end_lat   end_lng member_casual
    ## 247536                 41.88229 -87.63975 41.89000 -87.61000        casual
    ## 247537   KA1503000043  41.88212 -87.64005 41.88911 -87.63886        member
    ## 247538   TA1305000006  41.88196 -87.63995 41.88025 -87.62960        member
    ## 247539                 41.86000 -87.72000 41.85000 -87.71000        member
    ## 247540   TA1305000006  41.87785 -87.62408 41.88132 -87.62952        member

The rows correspond to each other in the original (raw) and new
dataframe. Let’s check if the general data structure has changed during
the merge process.

``` r
str(df_2021)
```

    ## 'data.frame':    5595063 obs. of  13 variables:
    ##  $ ride_id           : chr  "E19E6F1B8D4C42ED" "DC88F20C2C55F27F" "EC45C94683FE3F27" "4FA453A75AE377DB" ...
    ##  $ rideable_type     : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at        : chr  "2021-01-23 16:14:19" "2021-01-27 18:43:08" "2021-01-21 22:35:54" "2021-01-07 13:31:13" ...
    ##  $ ended_at          : chr  "2021-01-23 16:24:44" "2021-01-27 18:47:12" "2021-01-21 22:37:14" "2021-01-07 13:42:55" ...
    ##  $ start_station_name: chr  "California Ave & Cortez St" "California Ave & Cortez St" "California Ave & Cortez St" "California Ave & Cortez St" ...
    ##  $ start_station_id  : chr  "17660" "17660" "17660" "17660" ...
    ##  $ end_station_name  : chr  "" "" "" "" ...
    ##  $ end_station_id    : chr  "" "" "" "" ...
    ##  $ start_lat         : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ start_lng         : num  -87.7 -87.7 -87.7 -87.7 -87.7 ...
    ##  $ end_lat           : num  41.9 41.9 41.9 41.9 41.9 ...
    ##  $ end_lng           : num  -87.7 -87.7 -87.7 -87.7 -87.7 ...
    ##  $ member_casual     : chr  "member" "member" "member" "member" ...

The general data structure (data type and number of columns) in the new
date frame also corresponds to the original values.

With a high probability, we can say that the data was copied correctly
and now we have a large dataframe with all the data that we can start
analyzing and cleaning data.

### Data cleaning

At this stage, we will clear the resulting data array of redundant data,
incorrect and biased values. We convert the data into formats that are
more convenient for analysis. We will prepare our data so that at the
next stage we can do the analysis directly, so that nothing interferes
with us.

In our dataframe, all data can be conditionally grouped into several
groups, such as:</br>

-   User-trip - data about the trip and about the user: ride_id,
    rideable_type, member_causal;
-   Date and time - data about the time and date: started_at, ended_at ;
-   Location - data about the locations of the trip, and related data:
    start_station_name, start_station_id, end_station_name,
    end_station_id, start_lat, start_lng, end_lat, end_lng.

All three questions of our analysis are related to the study of the
behavior of users with a different type of membership in time, but not
in space. That is, we need to investigate the general behavior of users
depending on the time of the trip and the type of their membership, and
not on the metro station. Therefore, in the data to be analyzed, we must
get rid of the entire group of data associated with the location.
</br></br> New dataframe should consist from : ride_id, rideable_type,
started_at, ended_at , member_causal.</br>

Let’s do it.

``` r
# Creating new dataframe with only necessary columns 
df <- df_2021 %>% 
  select(ride_id,
         rideable_type,
         started_at,
         ended_at,
         member_casual)
```

Let’s check the data in the new dataframe and compare it with the
previous data structure.

``` r
head(df)
```

    ##            ride_id rideable_type          started_at            ended_at
    ## 1 E19E6F1B8D4C42ED electric_bike 2021-01-23 16:14:19 2021-01-23 16:24:44
    ## 2 DC88F20C2C55F27F electric_bike 2021-01-27 18:43:08 2021-01-27 18:47:12
    ## 3 EC45C94683FE3F27 electric_bike 2021-01-21 22:35:54 2021-01-21 22:37:14
    ## 4 4FA453A75AE377DB electric_bike 2021-01-07 13:31:13 2021-01-07 13:42:55
    ## 5 BE5E8EB4E7263A0B electric_bike 2021-01-23 02:24:02 2021-01-23 02:24:45
    ## 6 5D8969F88C773979 electric_bike 2021-01-09 14:24:07 2021-01-09 15:17:54
    ##   member_casual
    ## 1        member
    ## 2        member
    ## 3        member
    ## 4        member
    ## 5        casual
    ## 6        casual

``` r
str(df)
```

    ## 'data.frame':    5595063 obs. of  5 variables:
    ##  $ ride_id      : chr  "E19E6F1B8D4C42ED" "DC88F20C2C55F27F" "EC45C94683FE3F27" "4FA453A75AE377DB" ...
    ##  $ rideable_type: chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at   : chr  "2021-01-23 16:14:19" "2021-01-27 18:43:08" "2021-01-21 22:35:54" "2021-01-07 13:31:13" ...
    ##  $ ended_at     : chr  "2021-01-23 16:24:44" "2021-01-27 18:47:12" "2021-01-21 22:37:14" "2021-01-07 13:42:55" ...
    ##  $ member_casual: chr  "member" "member" "member" "member" ...

As we can see, the data coincide with the previous indicators. Namely,
the volume of rows, the number of columns, the types of data by column.

### Data conversion and data synthesis

We continue to prepare data for analysis and think about what types of
data we need in the dataframe. Now the data about the start and end time
of the trip is stored in the table as a “Character”, and it will be more
convenient for us when analyzing if we bring this data to datatime.

``` r
# Converting data type from chr to datetime
df[['started_at']] <- ymd_hms(df[['started_at']])
df[['ended_at']] <- ymd_hms(df[['ended_at']])
```

Checking data type converting results.

``` r
str(df)
```

    ## 'data.frame':    5595063 obs. of  5 variables:
    ##  $ ride_id      : chr  "E19E6F1B8D4C42ED" "DC88F20C2C55F27F" "EC45C94683FE3F27" "4FA453A75AE377DB" ...
    ##  $ rideable_type: chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ started_at   : POSIXct, format: "2021-01-23 16:14:19" "2021-01-27 18:43:08" ...
    ##  $ ended_at     : POSIXct, format: "2021-01-23 16:24:44" "2021-01-27 18:47:12" ...
    ##  $ member_casual: chr  "member" "member" "member" "member" ...

The next step is to improve the readability of column names.

``` r
# Renaming of columns and check
df <- df %>%
  rename(ride_type = rideable_type, 
         start_time = started_at,
         end_time = ended_at,
         customer_type = member_casual)
str(df)
```

    ## 'data.frame':    5595063 obs. of  5 variables:
    ##  $ ride_id      : chr  "E19E6F1B8D4C42ED" "DC88F20C2C55F27F" "EC45C94683FE3F27" "4FA453A75AE377DB" ...
    ##  $ ride_type    : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ start_time   : POSIXct, format: "2021-01-23 16:14:19" "2021-01-27 18:43:08" ...
    ##  $ end_time     : POSIXct, format: "2021-01-23 16:24:44" "2021-01-27 18:47:12" ...
    ##  $ customer_type: chr  "member" "member" "member" "member" ...

At the end of our data processing, we will consider what other data we
may need for analysis.

In our analysis, in order to understand exactly how different types of
users interact with our service at different times, it may be necessary
to group and compare data by month, day of the week and hours of day in
order to identify patterns of behavior.

We can do the calculation by the start_time and end_time fields every
time, but it is much easier to do it after analysis. Let’s do it.

``` r
# Adding day of the extracting weekday from start_time
df$ride_weekday <- format(as.Date(df$start_time),'%a')

# Adding ride_month extracting month from start_time
df$ride_month <- format(as.Date(df$start_time),'%b')

# Adding ride_time extracting hour of the day from start_time
df$ride_time <- df$start_time
df$ride_time <-format(as.POSIXct(df$ride_time),"%H")

# Checking results 
str(df)
```

    ## 'data.frame':    5595063 obs. of  8 variables:
    ##  $ ride_id      : chr  "E19E6F1B8D4C42ED" "DC88F20C2C55F27F" "EC45C94683FE3F27" "4FA453A75AE377DB" ...
    ##  $ ride_type    : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ start_time   : POSIXct, format: "2021-01-23 16:14:19" "2021-01-27 18:43:08" ...
    ##  $ end_time     : POSIXct, format: "2021-01-23 16:24:44" "2021-01-27 18:47:12" ...
    ##  $ customer_type: chr  "member" "member" "member" "member" ...
    ##  $ ride_weekday : chr  "Sat" "Wed" "Thu" "Thu" ...
    ##  $ ride_month   : chr  "Jan" "Jan" "Jan" "Jan" ...
    ##  $ ride_time    : chr  "16" "18" "22" "13" ...

We will also need the time of each trip, which we can calculate as the
difference between the end and the beginning of the trip.

``` r
#Adding ride_month calculation from start_time and end_time differences in minutes
df$ride_duration <- (as.double(difftime(df$end_time, df$start_time)))/60

str(df)
```

    ## 'data.frame':    5595063 obs. of  9 variables:
    ##  $ ride_id      : chr  "E19E6F1B8D4C42ED" "DC88F20C2C55F27F" "EC45C94683FE3F27" "4FA453A75AE377DB" ...
    ##  $ ride_type    : chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
    ##  $ start_time   : POSIXct, format: "2021-01-23 16:14:19" "2021-01-27 18:43:08" ...
    ##  $ end_time     : POSIXct, format: "2021-01-23 16:24:44" "2021-01-27 18:47:12" ...
    ##  $ customer_type: chr  "member" "member" "member" "member" ...
    ##  $ ride_weekday : chr  "Sat" "Wed" "Thu" "Thu" ...
    ##  $ ride_month   : chr  "Jan" "Jan" "Jan" "Jan" ...
    ##  $ ride_time    : chr  "16" "18" "22" "13" ...
    ##  $ ride_duration: num  10.417 4.067 1.333 11.7 0.717 ...

Let’s leave in the dataframe only those values that we need for further
analysis.

``` r
df <- df %>% 
  select(ride_id,
         ride_type,
         ride_time,
         ride_weekday, 
         ride_month,
         ride_duration,
         customer_type)
```

Let’s check the range of values for all columns in order to identify
data that may be erroneous and affect the analysis.

``` r
summary(df)
```

    ##    ride_id           ride_type          ride_time         ride_weekday      
    ##  Length:5595063     Length:5595063     Length:5595063     Length:5595063    
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##   ride_month        ride_duration      customer_type     
    ##  Length:5595063     Min.   :  -58.03   Length:5595063    
    ##  Class :character   1st Qu.:    6.75   Class :character  
    ##  Mode  :character   Median :   12.00   Mode  :character  
    ##                     Mean   :   21.94                     
    ##                     3rd Qu.:   21.78                     
    ##                     Max.   :55944.15

Significant deviations were detected in ride_duration, since the min
values are negative, which means there is a significant range of records
with incorrect data about the beginning and end of the trip.

They need to be cleaned out of the data.

Significant deviations were detected in ride_duration, since the min
values are negative, which means there is a significant range of records
with incorrect data about the beginning and end of the trip. Also,
looking at the maximum value of the duration of trips, it is clear that
there are outliers in the maximum direction, taking into account the
median values, I propose to limit the range of trip lengths considered
in the analysis to 6 hours.

All values below 0 minutes and above 360 must be cleaned from the data.

``` r
# Cleaning ride_duration and check 
df <- df[!(df$ride_duration < 0),]
df <- df[!(df$ride_duration > 60),]
summary(df)
```

    ##    ride_id           ride_type          ride_time         ride_weekday      
    ##  Length:5346509     Length:5346509     Length:5346509     Length:5346509    
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##   ride_month        ride_duration   customer_type     
    ##  Length:5346509     Min.   : 0.00   Length:5346509    
    ##  Class :character   1st Qu.: 6.55   Class :character  
    ##  Mode  :character   Median :11.43   Mode  :character  
    ##                     Mean   :14.86                     
    ##                     3rd Qu.:19.88                     
    ##                     Max.   :60.00

Let’s check the range of values for trip types and customer types to
make sure there are no values there that can distort our analysis.

``` r
unique(df$ride_type)
```

    ## [1] "electric_bike" "classic_bike"  "docked_bike"

``` r
unique(df$customer_type)
```

    ## [1] "member" "casual"

The values are in order, so we leave them unchanged.

At this stage, our work is finished, we have checked the structure and
integration of the data, collected the data into one dataframe,
processed the data types and added additional data representations for
analysis.

The last step of data processin is the preparation of the final version
of the dataframe and exporting cleaning data to .csv file which could be
used in other system for visualization or analysis.

``` r
# Creating new, fresh copy of dataset for backup and use 'df' dataframe for analysis
df_clean <- df
```

``` r
# Exporting cleaned data to .csv
write.csv(df, "rides_data2021_clean.csv", row.names = F)
```

Now that the data has been cleaned and prepared, we can proceed directly
to data analysis.

## Data analysis

This part of our project is the most crucial, as it provides answers to
the questions posed to us in this project. We will analyze the data
using tabular data and visualizations. The main task is to find answers,
the entire graphical part for analysis. Stakeholders can use this
document to check the course of thought, the main visualizations with
data for them will be duplicated on the Tableua.

Let’s start with a simple one with the number of trips by user
categories in general and their distribution in a period of a month.

``` r
# Simple data overview
table(df$customer_type)
```

    ## 
    ##  casual  member 
    ## 2304979 3041530

We see that we have more trips from customers with memberships, but
casual ones are not particularly lagging. Since we are very limited in
data on specific users, conversions from casual to ‘member’ users,
available inventory (bicycles) and finances, we can draw general
conclusions on user interaction with the service over time and make
recommendations based on this data.

**Output1:** it is necessary to collect and aggregate more data: users
id, costs of trips, available number of bikes by location , etc

Let’s estimate the number of trips by month to understand the overall
dynamics by period (year) and assume what data we need to analyze
further.

``` r
# Visualizing of total rides by month 
df %>%  
  group_by(customer_type, ride_month ) %>% 
  summarise(num_ride = n(), .groups = 'drop') %>%
  ggplot(aes(x =  ride_month, y = num_ride)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Count of rides by months of the year", x="Months", y="Count of rides")+
  scale_x_discrete(limits = c("Jan", "Feb", "Mar", "Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"))
```

![](capestone_files/figure-gfm/viz_total_bymonth-1.png)<!-- -->

The dynamics of trips significantly change throughout the year. There is
a pronounced high season from June to October, midseason from April to
May and November, December, and low season from December to February.

**Output2:** I think that with more detailed analysis, the low season
should be excluded since the data for this low period can significantly
distort the overall picture of the service in normal mode and lead to
incorrect conclusions.

Consider the same data and the same period, but with type of users.

``` r
# Visualizing of total rides by month and by usertype
df %>%  
  group_by(customer_type, ride_month ) %>% 
  summarise(num_ride = n(), .groups = 'drop') %>%
  ggplot(aes(x =  ride_month, y = num_ride, fill = customer_type)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Count of rides by usertype and months of the year", x="Months", y="Count of rides")+
  scale_x_discrete(limits = c("Jan", "Feb", "Mar", "Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"))
```

![](capestone_files/figure-gfm/viz_total_bymonth_byusertype-1.png)<!-- -->

Casual users are as active as possible in high season, it is this period
that is most favorable for marketing activities aimed at casual
customers since at this time their maximum concentration. But to clarify
this information, as stated above, it is necessary to analyze except low
season data.

**Output3:** Casual users are most active from May to October.

Let’s study the data by days of the week and types of users.

``` r
# Visualizing of total rides by weekday and by usertype
df %>%  
  group_by(customer_type, ride_weekday) %>% 
  summarise(num_ride = n(), .groups = 'drop') %>%
  ggplot(aes(x =  ride_weekday, y = num_ride, fill = customer_type)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Count of rides by usertype and day of the week", x="Weekdays", y="Count of rides") +
  scale_x_discrete(limits = c("Mon","Tue","Wed","Thu","Fri","Sat","Sun"))
```

![](capestone_files/figure-gfm/viz_total_byweekday_byusertype-1.png)<!-- -->

Casual users are also more active on weekends than on weekdays. And the
number of their trips exceeds the trips of regular users. This also
needs to be taken into account and rechecked when analyzing data only
for the middle and high seasons.

**Output4:** Casual users are more active on weekends. On other days,
their activity is significantly less.

Let’s analyze the general data on trips by the hour in an average day,
taking into account the type of user.

``` r
# Visualizing of total rides by time of the day and by usertype
df %>%  
  group_by(customer_type, ride_time) %>% 
  summarise(num_ride = n(), .groups = 'drop') %>%
  ggplot(aes(x =  ride_time, y = num_ride, fill = customer_type)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Count of rides by usertype and time of the day", x="Hours", y="Count of rides")
```

![](capestone_files/figure-gfm/viz_total_bytime_byusertype-1.png)<!-- -->
The graph clearly shows that regular users have two periods of activity,
this is in the morning from 6 am to 9 am, then active use up to 16 hours
and again peak activity up to 20 hours. As for casual users, their
activity starts at 10 o’clock and ends around 19 o’clock. It is
necessary to analyze these data by day of the week and on the data
without the low season’s data.

**Output5:** Casual users are active evenly throughout the day, regular
users have several peaks.

Now let’s analyze how our users use different types of bikes for their
trips.

``` r
# Visualizing type of bike usage by diffrent usertype
df %>%  
  group_by(customer_type, ride_type) %>% 
  summarise(num_ride = n(), .groups = 'drop') %>%
  ggplot(aes(x =  ride_type, y = num_ride, fill = customer_type)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Usage of diffrent type of bike by usertype", x="Bike type", y="Count of rides")
```

![](capestone_files/figure-gfm/viz_bike_usage_byusertype-1.png)<!-- -->

Members often use classic bicycles. Docked bikes are the most sought
after and used by both members and occasional riders. Electric bikes are
more popular with members. If e-bikes cost the most of all 3 classes, it
would be a good financial move to increase their fleet and at the same
time reduce docked bikes, because they already have priority over the
members who covered the most trips.

**Output6:**Casual users prefer electric and classic bikes

Now let’s prepare for a more detailed analysis without taking into
account the data of the low season, which significantly affects the
analysis. To do this, we will create a new data frame without data for
the period December - February and analyze the data of the high and mid
seasons.

``` r
# Removing all rows which contains low seasons values

df_hm <- df
df_hm <- df_hm[!(df_hm$ride_month == "Jan"),]
df_hm <- df_hm[!(df_hm$ride_month == "Feb"),]
df_hm <- df_hm[!(df_hm$ride_month == "Dec"),]
head(df_hm)
```

    ##                 ride_id    ride_type ride_time ride_weekday ride_month
    ## 146457 CFA86D4455AA1030 classic_bike        08          Tue        Mar
    ## 146458 30D9DC61227D1AF3 classic_bike        01          Sun        Mar
    ## 146459 846D87A15682A284 classic_bike        21          Thu        Mar
    ## 146460 994D05AA75A168F2 classic_bike        13          Thu        Mar
    ## 146461 DF7464FBE92D8308 classic_bike        09          Sun        Mar
    ## 146462 CEBA8516FD17F8D8 classic_bike        11          Sat        Mar
    ##        ride_duration customer_type
    ## 146457      4.066667        casual
    ## 146458     10.450000        casual
    ## 146459     16.400000        casual
    ## 146460     28.983333        casual
    ## 146461     17.933333        casual
    ## 146462     20.866667        casual

``` r
tail(df_hm)
```

    ##                  ride_id     ride_type ride_time ride_weekday ride_month
    ## 5347518 2E383B4D2965B154 electric_bike        16          Thu        Nov
    ## 5347519 E00E9F3500D69BAA electric_bike        00          Mon        Nov
    ## 5347520 8EAA66CE314E5FF1 electric_bike        13          Wed        Nov
    ## 5347521 36C2DC8BB1E13491 electric_bike        19          Tue        Nov
    ## 5347522 8E42FE5C67DF6A96 electric_bike        20          Wed        Nov
    ## 5347523 4F15069E2D2519BC electric_bike        20          Tue        Nov
    ##         ride_duration customer_type
    ## 5347518      9.283333        member
    ## 5347519     12.466667        member
    ## 5347520      4.900000        member
    ## 5347521      3.966667        member
    ## 5347522      6.916667        member
    ## 5347523     19.450000        member

We created a new data frame and checked the beginning and end of the
data to make sure that the low season data was cleared. Now we will look
at the data in various sections to find answers to the questions posed
to us.

Let’s look at the data on trips by month and day of the week, taking
into account the type of users, now with new data that exclude low
season data. For clarity, we will arrange the graphs with a grid.

``` r
# Visualizing rides by day of the week, months and usertype.
df_hm %>%  
  group_by(customer_type, ride_weekday, ride_month) %>% 
  summarise(num_ride = n(), .groups = 'drop') %>%
  ggplot(aes(x =  ride_weekday, y = num_ride, fill = customer_type)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Rides by usertype and day of the week by month", x="Weekdays", y="Count of rides")+
  scale_x_discrete(limits = c("Mon","Tue","Wed","Thu","Fri","Sat","Sun")) +
  theme(legend.position="bottom") +
  facet_wrap(~ ride_month)
```

![](capestone_files/figure-gfm/viz_total_byweekday_by_month_byusertype_hm-1.png)<!-- -->

Analyzing these data, we can see that casual user are very active on
weekends during the high season month (Jun-Sep), and less active during
the months of average demand. The rest of the time is dominated by
regular users.

**Output7:** Casual users are as active as possible during the weekends
of the high season months.

Let’s plot the dependence of the count of trips by the hour, day, and
month, as well as by user type.

``` r
# Visualizing rides by time of the day, months and usertype.
df_hm %>%  
  group_by(customer_type, ride_time, ride_month) %>% 
  summarise(num_ride = n(), .groups = 'drop') %>%
  ggplot(aes(x = ride_time, y = num_ride, fill = customer_type)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Rides by usertype and day of the week by month", x="Hours of the day", y="Count of rides")+
  theme(legend.position="bottom") +
  facet_wrap(~ ride_month)
```

![](capestone_files/figure-gfm/viz_total_bytime_by_month_byusertype_hm-1.png)<!-- -->

In almost all months, members have two periods of high activity: 6 am to
12 and from 16 to 20. And for casuals, the main time starts at 10-11 in
the morning, and in the evening hours most often far exceeds the number
of trips of members. This is especially pronounced during the high
season. In July and August, casual users from 10-11 am dominate until
the very end of the day.

**Output8:** Casuals are active during the daytime and evening hours in
July and August, as well as partially in June.

Up to this point, we have considered the total number of trips in
different slices and views, now we will proceed to the analysis of the
trip time expressed in median values.We will create and consider three
data sections available to us at once: by month, by day of the week, and
by the time of day. After that, we will conclude the entire group of
graphs

``` r
# Visualizing avarage rides time by months and usertype.
df_hm %>%  
  group_by(customer_type, ride_month) %>% 
  summarise(avg_ride = mean(ride_duration), .groups = 'drop') %>%
  ggplot(aes(x = ride_month, y = avg_ride, fill = customer_type)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Avg. rides time  by usertype by months")+
  theme(legend.position="bottom") +
  scale_x_discrete(limits = c("Mar", "Apr","May","Jun","Jul","Aug","Sep","Oct","Nov"))
```

![](capestone_files/figure-gfm/viz_avg_time_by_month_byusertype_hm-1.png)<!-- -->

``` r
# Visualizing avarage rides time by day of week and usertype.
df_hm  %>%  
  group_by(customer_type, ride_weekday, ride_month) %>% 
  summarise(avg_ride = mean(ride_duration), .groups = 'drop') %>%
  ggplot(aes(x =  ride_weekday, y = avg_ride, fill = customer_type)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Count of ride by user type and day of the week")+
  scale_x_discrete(limits = c("Mon","Tue","Wed","Thu","Fri","Sat","Sun")) 
```

![](capestone_files/figure-gfm/viz_avg_time_by_weekday_byusertype_hm%7D-1.png)<!-- -->

``` r
# Visualizing avarage rides time by day of week and usertype.
df_hm %>%  
  group_by(customer_type, ride_time) %>% 
  summarise(avg_ride = mean(ride_duration), .groups = 'drop') %>%
  ggplot(aes(x = ride_time, y = avg_ride, fill = customer_type)) +
  geom_col(width=0.5, position = position_dodge(width=0.5)) + 
  labs(title ="Rides by usertype and day of the week")+
  theme(legend.position="bottom")
```

![](capestone_files/figure-gfm/viz_avg_time_by_hours_byusertype_hm-1.png)<!-- -->

In all months, weekdays and hours of the average travel time of a casual
user is higher than that of a member user.

**Output9:** Casual users drive longer on average based on one ride.

## Conclusions and answers

The assignment for this study asked three questions:

1.  How do annual members and casual riders use Cyclistic bikes
    differently?
2.  Why would casual riders buy Cyclistic annual memberships?
3.  How can Cyclistic use digital media to influence casual riders to
    become members?

Having done the research, I have answers to two of them and a suggestion
in response to another one.

#### Answer 1.

I will answer these questions with theses about how casual users differ,
because given question number three, this group of users should be
analyzed more thoroughly. Here are some essential differences between
casual users:

1.  Casual users are most active from May to October. This is the high
    season for all types of users, but casual users are more active.
2.  Casual users are more active on weekends. On other days, their
    activity is significantly less.
3.  Casual users are active evenly throughout the day, regular users
    have several peaks.
4.  Casual users prefer electric and classic bikes e)Casual users are as
    active as possible during the weekends of the high season months.
5.  Casuals are active during the daytime and evening hours in July and
    August, as well as partially in June.
6.  Casual users drive longer on average based on one ride.

All these conclusions we can use to plan marketing activities aimed at
increasing the conversion of casual users into subscribers. These
findings should also be used to prepare for more in-depth analysis,
namely to start collecting more detailed data. But we’ll discuss that in
the Next Steps section.

#### Answer 2.

Within the framework of this research and the data that I had at my
disposal, I cannot unequivocally answer this question. Since it is
difficult to judge the motives of users to make purchases based on this
data. To answer this question, and not only for this, it will be
necessary to collect additional data and conduct research, which I will
write about in the Next Steps section.

#### Answer 3.

Judging by the data we have for the year, optimal marketing
communication with casual users is necessary during high season
weekends, when the service has the maximum concentration of such users
and such communication will get the highest return. But the first test
activities can be started on midseason weekends.

The answers about how to do this in a digital environment are also hard
to get from the available data. But obviously, the best way to set up a
campaign is to get the advertising message like this:

1.  Priority group of users: all users who have installed the service
    application, but do not have the status of a regular user and used
    the application at least once.
2.  Second group: users who have interacted with the service site in any
    way or made search queries about bike-sharing.
3.  All users who have installed the service’s application, but do not
    have the status of a regular user and do not use the application.

Additional data collection and analysis, which I will write about in the
next steps section will help you to elaborate on marketing activities in
more detail.

## Next steps

Taking into account all the conclusions obtained and not so much data, I
think it is necessary to propose to prepare for a deeper analysis of
users on the platform. To do this we need to collect more data in the
following moments of the platform:

Users: unique user ID on all activities in the system, changes in user
status in the system (fresh, made a trip, went to subscription,
unsubscribed).

Finances: the cost of each trip, the cost of the trip, the cost of
recruiting a new user, failed transactions.

Inventory: the number of available bikes at a time , the number of
available bikes per location.

External data: data about the current weather, data about forecast
variations, data about traffic jams.

This is basic data that we need to start understanding the actions of
the users on the platform. They will make it possible to build forecasts
and dependencies.

It will also be necessary to conduct qualitative research on the reasons
that motivate or demotivate users to subscribe or unsubscribe.

*Thank you for your time.*
