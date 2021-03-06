# R Cheatsheet

## Datamap = Map + Data + Viz
<a href="http://www.datamap.io">Datamap</a> is all about separation of concerns. 
- MAPS: No need to touch, you reference them by their universal GeoID, named id, in the GeoJSON/TopoJSON.
- DATA:
  - The REF file contains the id (= the global scope GeoID), names, codes etc.  
  - The EXT file contains your external data. You clean, wrangle, (bin) your data so that it can be matched and merged with the REF file.
  - The result = the DATA. After the merge of the REF and the EXT and the selection of desired columns, we have the DATA.
  - other reference files like e.g. CANDIDATE_REF usually start with what they reference, e.g. CANDIDATE.
- VIZ: Once you have the data and the map, you can start with the proper data visualization in D3 or import the map and data into services like Mapbox, CartoDB etc.

## Data Cleaning, Wrangling and Merging with R
Below you find a collection of code snippets which should help you with your data cleaning tasks.
In general we recommend to use the libraries of [Hadley Wickham](http://had.co.nz/), especially [dplyr](https://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html), [stringr](https://cran.r-project.org/web/packages/stringr/vignettes/stringr.html) and [resphape](http://seananderson.ca/2013/10/19/reshape.html).


## Set Working Directory
```
setwd("/Users/username/project")
## Check the current working directory with getwd(); similar to pwd (=print working directory) on the console
getwd()
```
## R Version and Upgrade
```
## Use the command "version"
version 

## Update R
1. Go to https://cran.cnr.berkeley.edu/ or to CRAN via https://www.r-project.org/
2. Download the latest version of R (currently 3.2.2)
3. Install
4. Restart RStudio

## Update RStudio
Go to Help > Check for Updates to install newer version.

## Update Packages
update.packages()
```

## Install Packages
```
install.packages("ggplot2")
library(ggplot2)

##Some problems with ggplot2 have been reported, this can be solved by installing lazyeval
install.packages(“lazyeval”)

##Installing Tidyverse (e.g. dplyr, purrr, tidyr, ggplot2)
install.packages("tidyverse")
library(tidyverse)
tidyverse_update()
```

## Downloading and reading files

### CSV
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
### Excel
NEW: Use [readxl](https://cran.r-project.org/web/packages/readxl/readxl.pdf), as it is much better in handling encodings.      
Just be aware that it works a bit differently than gdata.               

```        
## fileUrl = download URL; destfile = local file name, e.g. ext.xls
fileUrl <- "http://www.data.com/file.xls"            
download.file(fileUrl, destfile = "ext.xls", method = "curl")        

## Always document the time of the download as the file content or location might change        
dateDownloaded <- date()        
dateDownloaded           

## Install gdata
install.packages("readxl")           
library(readxl)       
          
excel_sheets("ext.xls")          
ext <- read_excel("ext.xls", skip=1, col_names = TRUE, sheet = "sheet1", stringsAsFactors = FALSE)         
```        
         
OLD: More about [gdata](http://www.r-bloggers.com/importing-data-directly-from-ms-excel/)

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
### TSV: Subset a dataframe by rows (see more about subsetting resp. selecting rows below)
```
## from row 8 to 1205 = 1197, with header=TRUE
ext <- read.table(file = 'ext20161206.tsv', sep="\t", header = TRUE, skip=7, nrows=1194)
```


### TopoJSON/GeoJSON
See more at: https://github.com/ropensci/geojsonio      
      
Install GDAL on the command line first, e.g., usingn homebrew   
```
brew install gdal
```
Then install rgdal and rgeos
```
install.packages("rgdal", type = "source", configure.args = "--with-gdal-config=/Library/Frameworks/GDAL.framework/Versions/1.11/unix/bin/gdal-config --with-proj-include=/Library/Frameworks/PROJ.framework/unix/include --with-proj-lib=/Library/Frameworks/PROJ.framework/unix/lib")
install.packages("rgeos", type = "source")
```
Install the stable version from CRAN
```
install.packages("geojsonio")
library(geojsonio)

## Read TopoJSON from URL
url <- "https://raw.githubusercontent.com/shawnbot/d3-cartogram/master/data/us-states.topojson"
tjson <- topojson_read(url, verbose = FALSE)

## Read TopoJSON from File (needs the suffix .topojson)
file <- "yourfile.topojson"
tjson <- topojson_read(file)
```

## Pattern Searching (grep, grepl)
```
## Searching in rows for "word" in Column 1 of data frame ext
search_pattern <- grepl(pattern = "word", x = ext$column1)
ext_searched <- ext[search_pattern, ]
```

## Change to numeric or to character
```
## Make column 2 to 11 numeric
ext8[, 2:11] <- sapply(ext8[, 2:11], as.numeric )

## Make id column as character
ext8$precinct_id <- sapply(ext8$precinct_id, as.character)
```
Alternative: Convert to numeric
```
## Choose cols to convert to numeric
cols <- ext2[, c(4:17)]

## Function convert.magic
convert.magic <- function(obj, type){
  FUN1 <- switch(type,
                 character = as.character,
                 numeric = as.numeric,
                 factor = as.factor)
  out <- lapply(obj, FUN1)
  as.data.frame(out)
}

## Convert and assign
ext2[, c(4:17)] <- convert.magic(cols, "numeric")
```

## Adding, pasting & trimming

### Add leading zeros
See [stringr](https://cran.r-project.org/web/packages/stringr/vignettes/stringr.html) for str_pad: 
```
library(stringr)
## Add a new variable county_fips to ext2 with leading zeros
ext$county_fips <- str_pad(ext2$FIPS.Code, width=3, pad = "0")
```

## Creating GeoID
```
## US = 840
## State FIPS = 2 numeric (e.g. 06)
## County FIPS = 3 numeric (e.g. 001)
## Result: 84006001
data$id <- paste("840", data$state_fips, data$county_fips, sep="")
```
### Put id first
```
## Put id as first column
newdf <- df %>% select(id, everything())
```

### Trim leading and trailing spaces

```
## trim function, see: gsub
trim <- function (x) gsub("^\\s+|\\s+$", "", x)

## apply trim function
df$column <- trim(df$column)
```

### Trim 3 trailing zeros
```
trim <- function (x) gsub("0{3}$", "", x)

## apply trim function to a new column
df$new_column <- trim(df$column)
```

### Removing the comma and transforming all characters to numbers (by column)
Entries in columns 3 to 9 are currently in the form "1,929" as characters. We want to remove the "," and make them numeric.
```
x <- ext[, c(3:9)] 

## 2 = columns
x <- apply(x, 2, function(y) as.numeric(gsub(",", "", y)))
ext[, c(3:9)] <- x

```
See also: https://www.datacamp.com/community/tutorials/r-tutorial-apply-family


## Comparing Dataframes or Columns of Datamframes
### Anti-join
From: [REF for Butte County](https://github.com/datamapio/US/blob/master/precinct/california/butte/REF/create_REF_butte_precinct.R)
```
## select the column to compare
library (plyr)
m <- map$PRECINCT
m <- ldply (m, data.frame)
pe <- preelec$regular_pct
pe <- ldply (pe, data.frame)

## compare from both sides, as you will get different results
require(dplyr) 
anti_join(m,pe)
anti_join(pe,m) 
```

And semi_join to filter rows in a1 that are also in a2
```
semi_join(a1,a2)
```

### Search for Duplicates
```
## First checking for uniques
map_unique <- unique(m) #545 instead of 563

## Then find the duplicates
duplicated <- duplicated(m)
d <- m[duplicated, ] #18
```


## Merging (horizontal) and Stacking (vertical)

### Checking for the difference of two dataframes before merging (with dplyr)
```
anti_join(a1,a2)
```
See also: [compare etc.](https://stackoverflow.com/questions/3171426/compare-two-data-frames-to-find-the-rows-in-data-frame-1-that-are-not-present-in)        

### Merge REF and EXT by common identifier (horizontal)
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

### Merge REF and EXT by common identifier, named differently in each data frame
```
data <- merge(ref, ext, by.x="gdenr", by.y="GMDNR", all=TRUE)
```



### Stacking data frames, one on top of the other (vertical)
```
AB_dfs <- rbind(df_A, df_B)
```
      
See also:      
http://www.princeton.edu/~otorres/Merge101R.pdf        


## Deleting, selecting, adding, renaming

### Deleting & selecting columns
dplyr: Select columns with select()   

There are a number of helper functions you can use within select(), like starts_with(), ends_with(), matches() and contains(). These let you quickly match larger blocks of variables that meet some criterion. See ?select for more details.

```
library(dplyr)

## Select columns by name
new_df <- select(df, column1, column2, column3, column5, column7, column10)

## Select all columns between column1 and column10 (inclusive)
new_df <- select(df, column1:column10)

## Select all columns except those from column1 to column3 (inclusive)
new_df <- select(flights, -(column1:column3))

```
Delete a column with <- NULL
```
df$column5 <- NULL
```
Or select columns by their position
```
## only keep columns1 to column3, column5 to column6 and column10
df_new <- df[ , c(1:3,5:6,10)]
```

### Deleting & selecting rows

Filter data with subset      
```
data_2010_2015 <- subset(data, year >= 2010 | year <= 2015)
```
   
Filter rows with filter() 
```
library(dplyr)
vbm <- filter(ext, grepl('VBM', reporting_type))
```


Selecting rows by position
```
library(dplyr)
ext <- slice(propf, 4:797)
```


Filter rows on specific names 
```
mtcars$type <- rownames(mtcars)
filter(mtcars, grepl('Toyota|Mazda', type))
```
Excluding rows with specific names
```
filter(mtcars, !grepl('Toyota|Mazda', type))
```


### Deleting rows by specific id, municipality_number, etc.
Create a vector with your numbers to delete
```
del_muni <- c("5011", "5098", "5101", "5106", "5110", "5116", "5123", "5127", "5128", "5134", "5153", "5165", "5217", "5223")
```
Delete the specific rows from your data frame
```
ref_nov <-ref_reduced[!(ref_reduced$municipality_number %in% del_muni), ] 
```

### Remove rows conditionally
```
## All rows with ZZ in the column cd114_fips are removed
ext <-ext[!(ext$cd114_fips =="ZZ"),]
```

## Ordering data frames
By default, sorting is ASCENDING. Prepend the sorting variable by a minus sign to indicate DESCENDING order.
It not always works for me, when I use the column name (eg. id). So I usually use df$id.
```
## ordering by id, ascending order
newdata <- df[order(df$id), ] 
## ordering by id, descending order
newdata <- df[order(-df$id), ] 
```


## Creating files
```
write.table(df, file="df.csv", sep="," ,col.names=TRUE, row.names=FALSE)

```


## Other Cheatsheets
http://shancarter.github.io/data-field-guide/           
https://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf           
## R Intros
Hands-on R class         
http://www.science.smith.edu/~amcnamara/NICAR2016.html            
http://www.r-tutor.com/elementary-statistics          
https://pagepiccinini.com/r-course/    
## R Manuals
https://cran.r-project.org/doc/manuals/r-release/R-intro.html      
## Regex
https://www.rstudio.com/wp-content/uploads/2016/09/RegExCheatsheet.pdf      
http://regex.danwin.com/slides/      
Regex101 - online regex editor and debugger            
https://regex101.com/
## R apply functions
https://rstudio-pubs-static.s3.amazonaws.com/84967_12ec13425c82452a8f357ba87a4f641f.html    

## Data Cleaning with R (Statistics Netherlands)
https://cran.r-project.org/doc/contrib/de_Jonge+van_der_Loo-Introduction_to_data_cleaning_with_R.pdf        

## dplyr Set operations (setdiff, union, intersect)
https://rdrr.io/cran/dplyr/man/setops.html            




