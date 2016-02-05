“us_five_largest_cities_2010_2014” because state fips should include leading zero, ie Alabama is 01, and in your file is just 1.


#R Cheatsheet

A collection of things you need to wrangle data.


##Set Working Directory
```
setwd("/Users/username/project"")
## Check the current working directory with getwd(); similar to pwd (=print working directory) on the console
getwd()
``

##Downloading and reading files

###CSV
```
fileUrl <- "http://www.data.com/file.csv"
download.file(fileUrl, destfile = "ext.csv", method = "curl")
## Check if file has been downloaded
list.files()

## Always document the time of the download as the file content or location might change
dateDownloaded <- date()
dateDownloaded

## Read csv in as data frame (df)
ext <- read.csv ("ext.csv", header = TRUE, sep = ",", stringsAsFactors=FALSE)
## Get the dimension of your data frame
dim(ext)
```
###Excel
More about [gdata](http://www.r-bloggers.com/importing-data-directly-from-ms-excel/)

```
## fileUrl = download URL; destfile = local file name, e.g. ext.xls
fileUrl <- "http://www.data.com/file.xls"
download.file(fileUrl, destfile = "ext.xls", method = "curl")

## Always document the time of the download as the file content or location might change
dateDownloaded <- date()
dateDownloaded

## Install gdata
install.packages("gdata")
library(gdata)
## Read first sheet in as data frame (df). 
## If the Excel has several sheets create ext1 for sheet=1, ext2 for sheet=2 etc.
ext <- read.xls ("ext.xls", sheet = 1, header = TRUE, stringsAsFactors=FALSE)

```


##Adding & Trimming

###Add leading zeros

###Trim leading and trailing spaces

```
## trim function, see: gsub
trim <- function (x) gsub("^\\s+|\\s+$", "", x)

## apply trim function
df$column <- trim(df$column)
```

##Merge REF and EXT by common identifier (horizontal)
```
## Adding the colums of ext to ref, ext and ref need to have the same identifier (id)
data <- merge(ref, ext, by="id", all=TRUE)
```
The merge() function allows four ways of combining data:
- Natural join: To keep only rows that match from the data frames, specify the argument all=FALSE.
- Full outer join: To keep all rows from both data frames, specify all=TRUE.
- Left outer join: To include all the rows of your data frame x and only those from y that match, specify all.x=TRUE.
- Right outer join: To include all the rows of your data frame y and only those from x that match, specify all.y=TRUE.

As the REF file contains not only the id, but also the state or county name, it makes most sense to keep all from both data frames and then get rid of the unneeded columns.

##Stacking data frames, one on top of the other (vertical)
```
two_dfs <- rbind(df_A, df_B)
```




