## Notes on the File Directory
All the data, maps, and overall steps I took to complete the final project are located in the Final Project GES687 folder of my Github repository.  This folder has three subfolders which are: Single Variable Maps, Bivariate Maps, Spatial Analysis Maps, and Data. The Single Variable Maps subfolder contains four maps that show the variables I used for this project; each map represents one variable. The Bivariate Maps subfolder has the three bivariate maps I made for this project. The Spatial Analysis Maps subfolder holds three maps that shows the spatial analyses I did with my variables. The Data subfolder includes all the raw and modified data from my project.  RStudio.Rmd and the index.md files contain the steps I used in Rstudio to make my initial maps and export them to QGIS; both files show these steps differently. Lastly, in the ReadMe.md file, I discuss the topic, the data, the transformations, analyses, and references of my project; the document, sample_pagefp.md, includes this as well. 

## Introduction
In this project, I made maps to see if there's a possible relationship between race/ethnicity and educational attainment in Puerto Rico. I made maps that show the percentages estimates and percentages of racial/ethnic groups in Puerto Rico; I also did this with Puerto Ricans with Bachelor's degrees. Then I created maps that display the distance between universities/colleges and majority African American/Black Puerto Rican populations.  I did the same thing with White and Asian Puerto Ricans.

## Data
I used data from the 2016 5-year American Community Survey and the Environmental Protection Agency's State Combined CSV Download Files. I downloaded the 2016 5-year ACS data in RSTudio which shows data between the years 2012 and 2016. I downloaded these variables from the 2016 5-year ACS: Native-Born White Puerto Ricans, Native-Born African American/Black Puerto Ricans, Native-Born Asian Puerto Ricans, total amount of Native-Born Puerto Ricans, total amount of Native-Born Puerto Ricans with a Bachelor's Degree, and total amount of Native-Born Puerto Ricans with some kind of educational attainment. The EPA's State Combined CSV Download Files contains zip files about every state and United States territory. These files have information on features like geospatial information and programs that are related to the EPA. I downloaded the EPA CSV file on Puerto Rico's facilities because it had information about the location of universities and colleges in the archipelago. 

## Transformations or Subsets
Before using the 2016 5-year ACS data to make the maps in RStudio, I made new variables which represent the percentages of Native-Born African American/Black Puerto Ricans,  
Native-Born White Puerto Ricans, Native-Born Asian Puerto Ricans, and Native-Born Puerto Ricans with a Bachelor's Degree. After making those variables, I made the single variable maps and the bivariate maps. Then I exported three versions of the RStudio data to make digital maps in QGIS; I turned the data into geojson files.  

### Exporting Output 1- African American/Black Bi_Class
```{r, eval= FALSE}
st_write(PR_Race_Edu16_Off2,  "C:/Users/lbayo/Documents/RStudio Test/FinalProject_BiMap_1.geojson")
```

### Exporting Output2- White Bi_Class
```{r, eval= FALSE}
st_write(PR_Race_Edu16_Off2,  "C:/Users/lbayo/Documents/RStudio Test/FinalProject_BiMap_2.geojson")
```

### Exporting Output 3- Asian Bi_Class
```{r, eval= FALSE}
st_write(PR_Race_Edu16_Off2,  "C:/Users/lbayo/Documents/RStudio Test/FinalProject_BiMap_3.geojson")
```

To make the single variable maps in QGIS, I included the geojson files as layers. The maps shows the estimates of Native-Born African American/Black Puerto Ricans, Native-Born White Puerto Ricans, Native-Born Asian Puerto Ricans, and Native-Born Puerto Ricans with a Bachelor's degree. The legend of each map has five classes and a graduated symbology. The mode of the classes is natural breaks (jenks); I chosed this mode because the data is skewed to one side and this mode reduces variation among the groups (Axis Maps 2020).

Then I began the process of making the three bivariate maps. These maps showed the relationship between belonging to a particular racial/ethnic group and having a Bachelor's degree in Puerto Rico. I made changes to each of the layer. I changed the value of each one to represent percentages and chose different color schemes. I also changed the number of classes in the legend to three. However, I still used a graduate symbology and with the mode natural breaks (jenks). Then I went to the Layer Rendering section and changed the Layer section to Multiply to show the color schemes of the racial/ethnic group and those with Bachelor's Degrees (Axis Maps 2020). For these maps, I used the Esri Light Gray Base Map from the HCMGIS plugin in QGIS. 

To use the EPA data, I first erased all facilities that weren't universities or colleges in Excel. Then I eliminated the universities and colleges that didn't include latitudes and longitudes in Excel as well. After making the final changes, I turned the updated EPA .csv file on Puerto Rico's facilites into a comma delimited text point layer.  The points represent the locations of the universities and colleges available. Then I used the Covert Geometry Type tool in QGIS to turn one of the geojson files into a centroid layer. The points in this layer represent the census tracts of the 2016 5-year ACS data. In the three spatial analysis maps, I showed the distance between locations with majority African American/Black, White, and Asian Puerto Ricans and the universities/colleges from the updated EPA .csv data. I used the Distance to Neareast Hub (Line to Hub) tool to make a layer that represents the distances in miles. Then I changed the value of this layer to HubDist and used a Graduated Symbology to represent the different distances; I added 5 classes and used the mode natural breaks (jenks). I used the centroid layer to show the different racial/ethnic groups. I used the percentages as values and used a Graduated Symbology as well. I applied the mode natural breaks (jenks) but added three classes. For each map, I used the Google Maps base map from the HCMGIS plugin in QGIS.

## Analysis and Interpretation 
The spatial analysis maps demonstrate interesting patterns between belonging to a particular racial/ethnic group and the physical location of universities and maps. The map that focused on Native-Born African American/Black Puerto Ricans show that majority African American/Black populations in the northeastern coast of the main island, which includes cities, are more likely to live near universities and colleges. However, majority African American/Black populations in the southeastern coast of the main island live farther from universities and colleges. 

The map that concentrated on Native-Born White Puerto Ricans show that majority White populations in the northeastern coast, counting the metropolitan area, live near universities and colleges as well. In addition, majority White populations who live really near the northwest and southwest coast of the main island live near universities and colleges. Yet, it seems that some majority White populations in rural areas, both in the east and west, live far from colleges and universities. There are even some majority White Populations right on the northwest and northeastern coasts that are far from colleges and universities. 

The map on Native-Born Asian Puerto Ricans indicate that majority Asian populations in the northeast, including the metropolitan area, and the northwest reside close to colleges and universities. But there is a majority Asian population in the southeast that lives far from universities and colleges as well. Lastly, there is a majority Asian population in the island of Vieques who has to take a ferryboat to get to the nearest university or college. 

## References
Axis Maps. 2020. “The Basics of Data Classification.” Retrieved May 21, 2021b (https://www.axismaps.com//guide/data-classification).

Alford, Natasha S. 2020. “Why Some Black Puerto Ricans Choose ‘White’ on the Census.” The New York Times, February 9. Retrieved May 21, 2021 from (https://www.nytimes.com/2020/02/09/us/puerto-rico-census-black-race.html#:~:text=All%20residents%20of%20Puerto%20Rico%20can%20select%20%E2%80%9CYes%2C,write%20something%20in.%20Most%20Puerto%20Ricans%20choose%20%E2%80%9Cwhite.%E2%80%9D)  

De Onis, Catalina M. 2018. “Energy Colonialism Powers the Ongoing Unnatural Disaster in Puerto Rico.” Frontiers in Communication 3(2):1–5. doi:10.3389/fcomm.2018.00002.

Goldsmith, Pat Rubio. 2009. “Schools or Neighborhoods or Both?: Race and Ethnic Segregation and Educational Attainment.” Social Forces 87(4):1913–41. doi: 10.1353/sof.0.0193.

History.COM Editors. 2017. “Puerto Rico.” History. Retrieved May 21, 2021 (https://www.history.com/topics/us-states/puerto-rico-history).

Nieuwenhuis, Jaap, Tom Kleinepier, and Maarten van Ham. 2021a. “The Role of Exposure to Neighborhood and School Poverty in Understanding Educational Attainment.” Journal of Youth and Adolescence 50(5):872–92. doi: 10.1007/s10964-021-01427-x.

Scheller, Mimi. 2018. “Caribbean Futures in the Offshore Anthropocene: Debt, Disaster, and Duration.” Environment and Planning D: Society and Space 36(6):971–86. doi: https://doi.org/10.1177/0263775818800849.

Trines, Stefan. 2018. “Economic Storm: The Crisis of Education in Puerto Rico.” WENR. Retrieved May 21, 2021 (https://wenr.wes.org/2018/05/economic-storm-the-crisis-of-education-in-puerto-rico).

United States Census Bureau. n.d. “Census Data API: /Data/2016/Acs/Acs5/Variables.” Retrieved May 21, 2021 (https://api.census.gov/data/2016/acs/acs5/variables.html).

United States Environmental Protection Agency. 2018. “EPA State Combined CSV Download Files.” US EPA. Retrieved May 21, 2021 (https://www.epa.gov/frs/epa-state-combined-csv-download-files).

Wickersham, Alice, Hannah Dickson, Rebecca Jones, Megan Pritchard, Robert Stewart, Tamsin Ford, and Johnny Downs. 2021. “Educational Attainment Trajectories among Children and Adolescents with Depression, and the Role of Sociodemographic Characteristics: Longitudinal Data-Linkage Study.” The British Journal of Psychiatry 218(3):151–57. doi: 10.1192/bjp.2020.160.
	



