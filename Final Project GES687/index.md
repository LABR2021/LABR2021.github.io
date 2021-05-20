---
title: "GES687 Final Project"
author: "Laura Bayona-Roman"
output: html_document
---
#### Installing Packages
```{r}
library(tidyverse)
library(tidycensus)
library(ggplot2)
theme_bw()
library(sf)
library(sp)
library(biscale)
library(cowplot)
```
#### Running the Census API key again
```{r}
options(tigris_class="sf")
options(tigris_use_cache=TRUE)
census_api_key("94e4f22691ce947e64eb28fd8e9c0ba431c4b343", overwrite=TRUE)
```
#### Getting Percentages for Race and Education (2016 5 year ACS Puerto Rico)
```{r}
PR_Race_Edu16_Off2 <- PR_Race_Edu16_Off2 %>% mutate(AfricanAmerican_Black_Percent =  PR_Pop_AfricanAmerican_BlackE / PR_Pop_NativeE * 100)
```

```{r}
PR_Race_Edu16_Off2 <- PR_Race_Edu16_Off2 %>% mutate(White_Percent =  PR_Pop_WhiteE / PR_Pop_NativeE * 100)
```

```{r}
PR_Race_Edu16_Off2 <- PR_Race_Edu16_Off2 %>% mutate(Asian_Percent =  PR_Pop_AsianE / PR_Pop_NativeE * 100)
```

```{r}
PR_Race_Edu16_Off2<- PR_Race_Edu16_Off2 %>% mutate(Bachelors_Percent =  PR_Pop_BachelorsE / PR_Pop_EducationE * 100)
```


#### Plotting Maps- Race
#### African American/Black (Purple HTML Colors)
```{r}
PR_Race_Edu16_Off2 %>% 
filter(PR_Pop_AfricanAmerican_BlackE != "NaN") %>%
ggplot() +
geom_sf(aes(fill= PR_Pop_AfricanAmerican_BlackE , color= PR_Pop_AfricanAmerican_BlackE)) + scale_fill_gradient(low= "#c67aff", high= "#540094", space= "Lab", na.value= "#7c00db", guide= "colourbar", aesthetics= "fill") + 
scale_color_gradient(low= "#c67aff", high= "#540094", space= "Lab", na.value= "#7c00db", guide= "colourbar", aesthetics= "colour") +
theme_bw()
```
#### White (Pink HTML Colors)
```{r}
PR_Race_Edu16_Off2 %>% 
  filter(PR_Pop_WhiteE != "NaN") %>% 
ggplot() +
geom_sf(aes(fill= PR_Pop_WhiteE, color= PR_Pop_WhiteE)) + scale_fill_gradient(low= "#ffe6f2", high= "#ff0080", space= "Lab", na.value= "#ff7abd", guide= "colourbar", aesthetics= "fill") + 
scale_color_gradient(low= "#ffe6f2", high= "#ff0080", space= "Lab", na.value= "#ff7abd", guide= "colourbar", aesthetics= "colour") +
theme_bw()
```
#### Asian (Orange HTML Colors)
```{r}
PR_Race_Edu16_Off2 %>% 
  filter(PR_Pop_AsianE != "NaN") %>% 
ggplot() +
geom_sf(aes(fill= PR_Pop_AsianE, color= PR_Pop_AsianE)) + scale_fill_gradient(low= "#ffd17a", high= "#946000", space= "Lab", na.value= "#f59f00", guide= "colourbar", aesthetics= "fill") + 
scale_color_gradient(low= "#ffd17a", high= "#946000", space= "Lab", na.value= "#f59f00", guide= "colourbar", aesthetics= "colour") +
theme_bw()
```
#### Plotting Maps Education
#### Bachelor's Degree (Olive HTML Colors)
```{r}
PR_Race_Edu16_Off2 %>% 
filter(PR_Pop_BachelorsE != "NaN") %>%
ggplot() +
geom_sf(aes(fill= PR_Pop_BachelorsE, color= PR_Pop_BachelorsE)) + scale_fill_gradient(low= "#fafa00", high= "#858500", space= "Lab", na.value= "#c2c200", guide= "colourbar", aesthetics= "fill") + 
scale_color_gradient(low= "#fafa00", high= "#858500", space= "Lab", na.value= "#c2c200", guide= "colourbar", aesthetics= "colour") +
theme_bw()
```
#### Plotting Bivariate Maps
#### African American/Black and Bachelor's Degree
```{r}
PR_Race_Edu16_Off2 <- bi_class(PR_Race_Edu16_Off2, x= AfricanAmerican_Black_Percent, y= Bachelors_Percent, style= "equal", dim= 3)
```


#### Filtering Out Missing Values from bi_class
```{r}
PR_Race_Edu16_Off2= PR_Race_Edu16_Off2%>%filter(bi_class != "NA-NA")
```


```{r}
Bivariate_RE_Off2.1 <- ggplot() +
  geom_sf(data= PR_Race_Edu16_Off2, mapping= aes(fill= bi_class), color= "white", size= 0.1, show.legend= FALSE) + 
  bi_scale_fill(pal= "DkViolet", dim= 3) +
  labs(
    title= "Race and Bachelor's Degree in Puerto Rico",
    subtile= "Race is African American/Black") +
  bi_theme() 

Bivariate_RE_Off2.1
```
```{r}
RE_PR_legend_Off2.1 <- bi_legend(pal = "DkViolet",
                           dim= 3,
                           xlab= "Higher % African American/Black",
                           ylab= "Higher % Bachelor's Degree %",
                           size= 7)
```

```{r}
ggdraw() + draw_plot(Bivariate_RE_Off2.1, 0, 0, 1, 1) + draw_plot(RE_PR_legend_Off2.1, 0.35, .70, 0.35, 0.35)
```
#### White and Bachelor's Degree
```{r}
PR_Race_Edu16_Off2 <- bi_class(PR_Race_Edu16_Off2, x= White_Percent, y= Bachelors_Percent, style= "equal", dim= 3)
```

```{r}
Bivariate_RE_Off3.1 <- ggplot() +
  geom_sf(data= PR_Race_Edu16_Off2, mapping= aes(fill= bi_class), color= "white", size= 0.1, show.legend= FALSE) + 
  bi_scale_fill(pal= "DkBlue", dim= 3) +
  labs(
    title= "Race and Bachelor's Degree in Puerto Rico",
    subtile= "Race is White") +
  bi_theme() 

Bivariate_RE_Off3.1
```{r}
RE_PR_legend_Off3.1 <- bi_legend(pal = "DkBlue",
                           dim= 3,
                           xlab= " Higher % White",
                           ylab= "Higher % Bachelor's Degree",
                           size= 7)
```

```{r}
ggdraw() + draw_plot(Bivariate_RE_Off3.1, 0, 0, 1, 1) + draw_plot(RE_PR_legend_Off3.1, 0.35, .70, 0.35, 0.35)
```
#### Asian and Bachelor's Degree
```{r}
PR_Race_Edu16_Off2 <- bi_class(PR_Race_Edu16_Off2, x= Asian_Percent, y= Bachelors_Percent, style= "equal", dim= 3)
```

```{r}
Bivariate_RE_Off4.1 <- ggplot() +
  geom_sf(data= PR_Race_Edu16_Off2, mapping= aes(fill= bi_class), color= "white", size= 0.1, show.legend= FALSE) + 
  bi_scale_fill(pal= "GrPink", dim= 3) +
  labs(
    title= "Race and Bachelor's Degree in Puerto Rico",
    subtile= "Race is Asian") +
  bi_theme() 

Bivariate_RE_Off4.1
```
```{r}
RE_PR_legend_Off4.1 <- bi_legend(pal = "GrPink",
                           dim= 3,
                           xlab= " Higher % Asian",
                           ylab= "Higher % Bachelor's Degree",
                           size= 7)
```

```{r}
ggdraw() + draw_plot(Bivariate_RE_Off4.1, 0, 0, 1, 1) + draw_plot(RE_PR_legend_Off4.1, 0.35, .70, 0.35, 0.35)
```
#### Checking the Coordinate Reference System
```{r}
st_crs(PR_Race_Edu16_Off2)
```
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
#### Turning dataframe into CSV file
```{r, eval= FALSE}
st_write(PR_Race_Edu16_Off2, "PR_Race_Edu16_Off2.csv") 
```
