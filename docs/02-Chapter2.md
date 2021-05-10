# Rstudio
This chapter contains syntax for manipulating data and packages within the  
R studio environment.

## Useful packages
Load all libraries

```r
library(tidyverse) # data handling and viz
library(janitor) #dataframe import cleaning
```

```
## Warning: package 'janitor' was built under R version 4.0.5
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```r
library(knitr) #nice html tables
```

```
## Warning: package 'knitr' was built under R version 4.0.5
```

```r
library(kableExtra) # nicer knitr tables
```

```
## Warning: package 'kableExtra' was built under R version 4.0.5
```

```
## 
## Attaching package: 'kableExtra'
```

```
## The following object is masked from 'package:dplyr':
## 
##     group_rows
```

```r
library(broom)
```

```
## Warning: package 'broom' was built under R version 4.0.5
```

```r
library(readr) # load csv stored data
library(geosphere) # for calc daylength
```

```
## Warning: package 'geosphere' was built under R version 4.0.5
```
## Remove a package

```r
#Unload a module: 
library(clipr) #load
```

```
## Welcome to clipr. See ?write_clip for advisories on writing to the clipboard in R.
```

```r
detach(package:clipr) #unload
```


## Import using Janitor

```r
# Create a data.frame with dirty names
test_df <- as.data.frame(matrix(ncol = 6))
names(test_df) <- c("firstName", "ábc@!*", "% successful (2009)",
                    "REPEAT VALUE", "REPEAT VALUE", "")
head(test_df)
test_df <- test_df %>%
  clean_names()
head(test_df)
```

```
##   firstName ábc@!* % successful (2009) REPEAT VALUE REPEAT VALUE   
## 1        NA     NA                  NA           NA           NA NA
##   first_name abc percent_successful_2009 repeat_value repeat_value_2  x
## 1         NA  NA                      NA           NA             NA NA
```
Reference [here](https://cran.r-project.org/web/packages/janitor/vignettes/janitor.html)

## Remove dataframe 

```r
data("mtcars")
data("band_instruments")
data("band_instruments2") # Load example datasets

rm(list=ls()[! ls() %in% c("band_instruments","band_instruments2")]) # Everything except Band instruments
rm(list=setdiff(ls(), "band_instruments")) # Everything except "bandinstruments"
rm(list=ls()) # Remove everything
```
Reference:
[Stackoverflow](https://stackoverflow.com/questions/6190051/how-can-i-remove-all-objects-but-one-from-the-workspace-in-r)

## Save dataframe  (CSV or Rdata)
*make date string*

```r
datenow <- format(Sys.time(), "_%Y_%m_%d")
date
data(mtcars)
```

```
## function () 
## .Internal(date())
## <bytecode: 0x000000002192b1d0>
## <environment: namespace:base>
```
*Write file names*

```r
#create data directory
dir.create("data_out")
```

```
## Warning in dir.create("data_out"): 'data_out' already exists
```

```r
filenamecsv <- paste("data_out/mtcsvdata",datenow,".csv",sep="")
filenamerda <- paste("data_out/mtrdadata",datenow,".rda",sep="")
```
*Save the files*

```r
save(mtcars,file=filenamerda)
write.csv(mtcars,file=filenamecsv)
```

## Save a diagram or plot

```r
plot1 <- mtcars %>% ggplot(aes(hp,qsec))+geom_point()
#plot1 #print plot if required
pdf("plot.pdf")
plot1
dev.off()
```

```
## pdf 
##   2
```
pdf 'device' off.

## Recode a text variable

```r
data("band_members")
kable(head(band_members)) %>% kable_minimal(full_width = F)
band_members <- band_members %>% mutate(name=recode(name, "Mick"= "m"))
kable(head((band_members))) %>% kable_minimal(full_width = F)
rm(list=ls()) # Remove everything
```

\begin{table}
\centering
\begin{tabular}{l|l}
\hline
name & band\\
\hline
Mick & Stones\\
\hline
John & Beatles\\
\hline
Paul & Beatles\\
\hline
\end{tabular}
\end{table}

\begin{table}
\centering
\begin{tabular}{l|l}
\hline
name & band\\
\hline
m & Stones\\
\hline
John & Beatles\\
\hline
Paul & Beatles\\
\hline
\end{tabular}
\end{table}
Reference: [Kable Extra](https://cran.r-project.org/web/packages/kableExtra/vignettes/awesome_table_in_html.html)

## Alter variable names:
*Remove underscores*

```r
data("mtcars")
mtcars <- mtcars %>% rename(hp_new=hp)
kable(head((mtcars))) %>% kable_minimal(full_width = F)

mtcars <- mtcars %>% rename_with(.fn = ~str_replace(., "_", ""))
kable(head((mtcars))) %>% kable_minimal(full_width = F)
```

\begin{table}
\centering
\begin{tabular}{l|r|r|r|r|r|r|r|r|r|r|r}
\hline
  & mpg & cyl & disp & hp\_new & drat & wt & qsec & vs & am & gear & carb\\
\hline
Mazda RX4 & 21.0 & 6 & 160 & 110 & 3.90 & 2.620 & 16.46 & 0 & 1 & 4 & 4\\
\hline
Mazda RX4 Wag & 21.0 & 6 & 160 & 110 & 3.90 & 2.875 & 17.02 & 0 & 1 & 4 & 4\\
\hline
Datsun 710 & 22.8 & 4 & 108 & 93 & 3.85 & 2.320 & 18.61 & 1 & 1 & 4 & 1\\
\hline
Hornet 4 Drive & 21.4 & 6 & 258 & 110 & 3.08 & 3.215 & 19.44 & 1 & 0 & 3 & 1\\
\hline
Hornet Sportabout & 18.7 & 8 & 360 & 175 & 3.15 & 3.440 & 17.02 & 0 & 0 & 3 & 2\\
\hline
Valiant & 18.1 & 6 & 225 & 105 & 2.76 & 3.460 & 20.22 & 1 & 0 & 3 & 1\\
\hline
\end{tabular}
\end{table}

\begin{table}
\centering
\begin{tabular}{l|r|r|r|r|r|r|r|r|r|r|r}
\hline
  & mpg & cyl & disp & hpnew & drat & wt & qsec & vs & am & gear & carb\\
\hline
Mazda RX4 & 21.0 & 6 & 160 & 110 & 3.90 & 2.620 & 16.46 & 0 & 1 & 4 & 4\\
\hline
Mazda RX4 Wag & 21.0 & 6 & 160 & 110 & 3.90 & 2.875 & 17.02 & 0 & 1 & 4 & 4\\
\hline
Datsun 710 & 22.8 & 4 & 108 & 93 & 3.85 & 2.320 & 18.61 & 1 & 1 & 4 & 1\\
\hline
Hornet 4 Drive & 21.4 & 6 & 258 & 110 & 3.08 & 3.215 & 19.44 & 1 & 0 & 3 & 1\\
\hline
Hornet Sportabout & 18.7 & 8 & 360 & 175 & 3.15 & 3.440 & 17.02 & 0 & 0 & 3 & 2\\
\hline
Valiant & 18.1 & 6 & 225 & 105 & 2.76 & 3.460 & 20.22 & 1 & 0 & 3 & 1\\
\hline
\end{tabular}
\end{table}

```r
plot(cars)
```

\begin{figure}

{\centering \includegraphics[width=1\linewidth]{02-Chapter2_files/figure-latex/unnamed-chunk-11-1} 

}

\caption{**CAPTION THIS FIGURE!!**}(\#fig:unnamed-chunk-11)
\end{figure}

