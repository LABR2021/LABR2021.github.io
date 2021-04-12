# GES687 Lab 6

## Bivariate Choropleth Map Showing Changes Between 2008 and 2013

## Important Notes About the File Directory
All the information about this lab is located in the GES_Lab6 folder of my Github repository. This folder contains a subfolder named Data where I uploaded all the raw and altered data. Moreover, I created an .Rmd file called RStudioPro.Rmd; this contains all the RStudio code I used to download the raw data, merge the data, and export it to QGIS. The index.md I made also contains the Rstudio code but it has a different format from the RStudioPro.Rmd.  I also have a folder named Bin that containg merged data files.

## Introduction
The topic of my map is the relationship between the presence of Puerto Ricans and housing vacancies in the state of Florida.  

## Data
The data came from the 2010 and 2013 3 year American Community Survey.  The 2010 survey contains data between the years of 2008 and 2010. The 2013 data contains data between the years of 2011 and 2013. I used these two packages from Rstudio to download the data: tidyverse and tidycensus. I accessed these links to choose the variables from the surveys: https://api.census.gov/data/2010/acs/acs3/profile/variables.html and https://api.census.gov/data/2013/acs/acs3/profile/variables.html. The files that contain raw data are: PR2013.geojson, PR2010.cvs, HVacant2013.geojson, and HVacant2010.csv. The file with altered data are: PR_Florida_per and HVacant_Florida_per. 

## Transformations or subsets
I changed the format of the raw data on Puerto Ricans and housing vacancies (from the 2013 3 year ACS survey). This data is now located in geojson files with a coordinate reference system of 3857.
```{r}
st_write(st_transform(PR2013per,3857), "PR2013per.geojson")
```
```{r}
st_write(st_transform(HVacant2013per,3857), "HVacant2013per.geojson")
```
I changed the format of the raw data on Puerto Ricans and housing vacancies (from the 2010 3 year ACS survey). The data is now located in .csv files. 
```{r}
st_write(PR2010per, "PR2010per.csv") 
```
```{r}
st_write(HVacant2010per, "HVacant2010per.csv") 
```

I merged the 2010 and 2013 data on Puerto Ricans using the function left_join.
```{r}
PR_Florida_per <- left_join(PR2013per, PR2010per,
                        by = "GEOID",
                        copy = FALSE,
                        suffix=c(".13",".10"))
```
I merged the 2010 and 2013 data on housing vacancies using the function left_join as well.
```{r}
HVacant_Florida_per <- left_join (HVacant2013per,  HVacant2010per,
                              by = "GEOID",
                              copy = FALSE,
                              suffix = c(".13", ".10"))
```

## Analysis
After merging the 2010 and 2013 data, I exported each altered variable to QGIS.
```{r}
st_write(PR_Florida_per, "PR_Florida_per.geojson")
```

```{r}
st_write(HVacant_Florida_per, "HVacant_Florida_per.geojson")
```

In QGIS, I downloaded each layer to make the map that shows. Then I used the graduate symbology to represent changes in time. 

## Outputs
My output is a bivariate, choropleth map demonstrating the asssociation between the presence of Puerto Ricans and housing vacancies by counties in Florida (these changes are indicated by percentages). This map is related to the content of this class because I used geographic data and software to analyze the link between two variables that might significantly affect a specific population. 
