---
title: 'Weekly Exercises #4'
author: "Yunyang Zhong"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---




```r
library(tidyverse)     # for data cleaning and plotting
```

```
## Warning: package 'tidyverse' was built under R version 4.0.5
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.3     v purrr   0.3.4
## v tibble  3.1.1     v dplyr   1.0.6
## v tidyr   1.1.3     v stringr 1.4.0
## v readr   1.4.0     v forcats 0.5.1
```

```
## Warning: package 'ggplot2' was built under R version 4.0.5
```

```
## Warning: package 'tibble' was built under R version 4.0.5
```

```
## Warning: package 'tidyr' was built under R version 4.0.5
```

```
## Warning: package 'purrr' was built under R version 4.0.4
```

```
## Warning: package 'dplyr' was built under R version 4.0.5
```

```
## Warning: package 'forcats' was built under R version 4.0.5
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(lubridate)     # for date manipulation
```

```
## Warning: package 'lubridate' was built under R version 4.0.5
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(openintro)     # for the abbr2state() function
```

```
## Warning: package 'openintro' was built under R version 4.0.5
```

```
## Loading required package: airports
```

```
## Warning: package 'airports' was built under R version 4.0.5
```

```
## Loading required package: cherryblossom
```

```
## Warning: package 'cherryblossom' was built under R version 4.0.5
```

```
## Loading required package: usdata
```

```
## Warning: package 'usdata' was built under R version 4.0.5
```

```r
library(palmerpenguins)# for Palmer penguin data
```

```
## Warning: package 'palmerpenguins' was built under R version 4.0.5
```

```r
library(maps)          # for map data
```

```
## 
## Attaching package: 'maps'
```

```
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
library(ggmap)         # for mapping points on maps
```

```
## Warning: package 'ggmap' was built under R version 4.0.5
```

```
## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.
```

```
## Please cite ggmap if you use it! See citation("ggmap") for details.
```

```r
library(gplots)        # for col2hex() function
```

```
## Warning: package 'gplots' was built under R version 4.0.5
```

```
## 
## Attaching package: 'gplots'
```

```
## The following object is masked from 'package:stats':
## 
##     lowess
```

```r
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
```

```
## Warning: package 'sf' was built under R version 4.0.5
```

```
## Linking to GEOS 3.9.0, GDAL 3.2.1, PROJ 7.2.1
```

```r
library(leaflet)       # for highly customizable mapping
```

```
## Warning: package 'leaflet' was built under R version 4.0.5
```

```r
library(carData)       # for Minneapolis police stops data
library(ggthemes)      # for more themes (including theme_map())
```

```
## Warning: package 'ggthemes' was built under R version 4.0.5
```

```r
theme_set(theme_minimal())
```


```r
# Starbucks locations
Starbucks <- read_csv("https://www.macalester.edu/~ajohns24/Data/Starbucks.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Brand = col_character(),
##   `Store Number` = col_character(),
##   `Store Name` = col_character(),
##   `Ownership Type` = col_character(),
##   `Street Address` = col_character(),
##   City = col_character(),
##   `State/Province` = col_character(),
##   Country = col_character(),
##   Postcode = col_character(),
##   `Phone Number` = col_character(),
##   Timezone = col_character(),
##   Longitude = col_double(),
##   Latitude = col_double()
## )
```

```r
starbucks_us_by_state <- Starbucks %>% 
  filter(Country == "US") %>% 
  count(`State/Province`) %>% 
  mutate(state_name = str_to_lower(abbr2state(`State/Province`))) 

# Lisa's favorite St. Paul places - example for you to create your own data
favorite_stp_by_lisa <- tibble(
  place = c("Home", "Macalester College", "Adams Spanish Immersion", 
            "Spirit Gymnastics", "Bama & Bapa", "Now Bikes",
            "Dance Spectrum", "Pizza Luce", "Brunson's"),
  long = c(-93.1405743, -93.1712321, -93.1451796, 
           -93.1650563, -93.1542883, -93.1696608, 
           -93.1393172, -93.1524256, -93.0753863),
  lat = c(44.950576, 44.9378965, 44.9237914,
          44.9654609, 44.9295072, 44.9436813, 
          44.9399922, 44.9468848, 44.9700727)
  )

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   date = col_date(format = ""),
##   state = col_character(),
##   fips = col_character(),
##   cases = col_double(),
##   deaths = col_double()
## )
```

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

These exercises will reiterate what you learned in the "Mapping data with R" tutorial. If you haven't gone through the tutorial yet, you should do that first.

### Starbucks locations (`ggmap`)

  1. Add the `Starbucks` locations to a world map. Add an aesthetic to the world map that sets the color of the points according to the ownership type. What, if anything, can you deduce from this visualization?  


```r
world <- get_stamenmap(
    bbox = c(left = -180, bottom = -57, right = 179, top = 82.1), 
    maptype = "terrain",
    zoom = 2)
```

```
## Source : http://tile.stamen.com/terrain/2/0/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/0/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/0/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/2.png
```

```r
ggmap(world) + 
  geom_point(data = Starbucks, 
             aes(x = Longitude, y = Latitude, color = `Ownership Type`), 
             alpha = .5, 
             size = .1) +
  theme_map() +
  theme(legend.background = element_blank())
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

> Most Starbucks in the U.S. are ????

  2. Construct a new map of Starbucks locations in the Twin Cities metro area (approximately the 5 county metro area).  


```r
starbucks_tc <- Starbucks %>% 
  filter(Country == "US",
         `State/Province` == "MN",
         City %in% c("Minneapolis", "St. Paul"))

tc <- get_stamenmap(
    bbox = c(left = -93.6, bottom = 44.8, right = -92.8, top = 45.1), 
    maptype = "terrain",
    zoom = 11)
```

```
## Source : http://tile.stamen.com/terrain/11/491/735.png
```

```
## Source : http://tile.stamen.com/terrain/11/492/735.png
```

```
## Source : http://tile.stamen.com/terrain/11/493/735.png
```

```
## Source : http://tile.stamen.com/terrain/11/494/735.png
```

```
## Source : http://tile.stamen.com/terrain/11/495/735.png
```

```
## Source : http://tile.stamen.com/terrain/11/496/735.png
```

```
## Source : http://tile.stamen.com/terrain/11/491/736.png
```

```
## Source : http://tile.stamen.com/terrain/11/492/736.png
```

```
## Source : http://tile.stamen.com/terrain/11/493/736.png
```

```
## Source : http://tile.stamen.com/terrain/11/494/736.png
```

```
## Source : http://tile.stamen.com/terrain/11/495/736.png
```

```
## Source : http://tile.stamen.com/terrain/11/496/736.png
```

```
## Source : http://tile.stamen.com/terrain/11/491/737.png
```

```
## Source : http://tile.stamen.com/terrain/11/492/737.png
```

```
## Source : http://tile.stamen.com/terrain/11/493/737.png
```

```
## Source : http://tile.stamen.com/terrain/11/494/737.png
```

```
## Source : http://tile.stamen.com/terrain/11/495/737.png
```

```
## Source : http://tile.stamen.com/terrain/11/496/737.png
```

```
## Source : http://tile.stamen.com/terrain/11/491/738.png
```

```
## Source : http://tile.stamen.com/terrain/11/492/738.png
```

```
## Source : http://tile.stamen.com/terrain/11/493/738.png
```

```
## Source : http://tile.stamen.com/terrain/11/494/738.png
```

```
## Source : http://tile.stamen.com/terrain/11/495/738.png
```

```
## Source : http://tile.stamen.com/terrain/11/496/738.png
```

```r
ggmap(tc) + 
  geom_point(data = starbucks_tc, 
             aes(x = Longitude, y = Latitude, color = `Ownership Type`), 
             alpha = 1, 
             size = 1) +
  theme_map() +
  theme(legend.background = element_blank())
```

![](04_exercises_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

  3. In the Twin Cities plot, play with the zoom number. What does it do?  (just describe what it does - don't actually include more than one map).  

> It controls how detailed the plot would be.

  4. Try a couple different map types (see `get_stamenmap()` in help and look at `maptype`). Include a map with one of the other map types.  


```r
tc_wc <- get_stamenmap(
    bbox = c(left = -93.6, bottom = 44.8, right = -92.8, top = 45.1), 
    maptype = "watercolor",
    zoom = 11)
```

```
## Source : http://tile.stamen.com/watercolor/11/491/735.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/492/735.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/493/735.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/494/735.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/495/735.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/496/735.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/491/736.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/492/736.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/493/736.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/494/736.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/495/736.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/496/736.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/491/737.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/492/737.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/493/737.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/494/737.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/495/737.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/496/737.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/491/738.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/492/738.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/493/738.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/494/738.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/495/738.jpg
```

```
## Source : http://tile.stamen.com/watercolor/11/496/738.jpg
```

```r
ggmap(tc_wc) + 
  geom_point(data = starbucks_tc, 
             aes(x = Longitude, y = Latitude, color = `Ownership Type`), 
             alpha = 1, 
             size = 1) +
  theme_map() +
  theme(legend.background = element_blank())
```

![](04_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. Add a point to the map that indicates Macalester College and label it appropriately. There are many ways you can do think, but I think it's easiest with the `annotate()` function (see `ggplot2` cheatsheet).


```r
mac <- get_stamenmap(
    bbox = c(left = -93.18, bottom = 44.93, right = -93.16, top = 44.94), 
    maptype = "terrain",
    zoom = 16)
```

```
## Source : http://tile.stamen.com/terrain/16/15805/23590.png
```

```
## Source : http://tile.stamen.com/terrain/16/15806/23590.png
```

```
## Source : http://tile.stamen.com/terrain/16/15807/23590.png
```

```
## Source : http://tile.stamen.com/terrain/16/15808/23590.png
```

```
## Source : http://tile.stamen.com/terrain/16/15805/23591.png
```

```
## Source : http://tile.stamen.com/terrain/16/15806/23591.png
```

```
## Source : http://tile.stamen.com/terrain/16/15807/23591.png
```

```
## Source : http://tile.stamen.com/terrain/16/15808/23591.png
```

```
## Source : http://tile.stamen.com/terrain/16/15805/23592.png
```

```
## Source : http://tile.stamen.com/terrain/16/15806/23592.png
```

```
## Source : http://tile.stamen.com/terrain/16/15807/23592.png
```

```
## Source : http://tile.stamen.com/terrain/16/15808/23592.png
```

```r
ggmap(mac) + 
  geom_point(aes(x = 44.9379, y = -93.1691), 
             alpha = 1, 
             size = 1) +
  theme_map() +
  annotate(geom = "point", x = 44.9379, y = -93.1691, label = "Macalester College")
```

```
## Warning: Ignoring unknown parameters: label
```

```
## Warning: Removed 4 rows containing missing values (geom_point).
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

### Choropleth maps with Starbucks data (`geom_map()`)

The example I showed in the tutorial did not account for population of each state in the map. In the code below, a new variable is created, `starbucks_per_10000`, that gives the number of Starbucks per 10,000 people. It is in the `starbucks_with_2018_pop_est` dataset.


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   state = col_character(),
##   est_pop_2018 = col_double()
## )
```

```r
starbucks_with_2018_pop_est <-
  starbucks_us_by_state %>% 
  left_join(census_pop_est_2018,
            by = c("state_name" = "state")) %>%
  mutate(starbucks_per_10000 = (n/est_pop_2018)*10000)
```

  6. **`dplyr` review**: Look through the code above and describe what each line of code does.

> We first read in the data. Because under the state column, each state has an extra dot before the name, we separate the name into a dot and the actual state name and name them accordingly. Then, the dot column is removed, and the state column is replaced with all lower case letters. In the second chunk, the census dataset is combined with the Starbuck dataset; all entries in the Starbuck dataset are kept, and entries in the census dataset with a corresponding state name in the Starbuck dataset are also kept. The last line creates a new variable representing the number of Starbucks per 10,000 population.

  7. Create a choropleth map that shows the number of Starbucks per 10,000 people on a map of the US. Use a new fill color, add points for all Starbucks in the US (except Hawaii and Alaska), add an informative title for the plot, and include a caption that says who created the plot (you!). Make a conclusion about what you observe.


```r
states_map <- map_data("state")

starbucks_with_2018_pop_est %>% 
  filter(state_name != "hawaii" & state_name != "alaska") %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state_name,
               fill = starbucks_per_10000)) +
  geom_point(data = Starbucks %>% 
               filter(Country == "US") %>% 
               filter(`State/Province` != "HI" & `State/Province` != "AK"),
             aes(x = Longitude, y = Latitude),
             size = .05,
             alpha = .2, 
             color = "goldenrod") +
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map() +
  labs(title = "Starbucks in the US",
       caption = "Plot created by Yunyang Zhong")
```

![](04_exercises_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

> ????

### A few of your favorite things (`leaflet`)

  8. In this exercise, you are going to create a single map of some of your favorite places! The end result will be one map that satisfies the criteria below. 

  * Create a data set using the `tibble()` function that has 10-15 rows of your favorite places. The columns will be the name of the location, the latitude, the longitude, and a column that indicates if it is in your top 3 favorite locations or not. For an example of how to use `tibble()`, look at the `favorite_stp_by_lisa` I created in the data R code chunk at the beginning.  

  * Create a `leaflet` map that uses circles to indicate your favorite places. Label them with the name of the place. Choose the base map you like best. Color your 3 favorite places differently than the ones that are not in your top 3 (HINT: `colorFactor()`). Add a legend that explains what the colors mean.  
  
  * Connect all your locations together with a line in a meaningful way (you may need to order them differently in the original data).  
  
  * If there are other variables you want to add that could enhance your plot, do that now.  


```r
fav_yunyang <- tibble(
  place = c("Home", "Macalester College", "Shish", 
            "Nelson's Ice Cream", "Kbop Korean Bistro", "MOA",
            "MIA", "Tasty Pot", "Toppers Pizza", "Legendary Spice"),
  long = c(-93.175608887381, -93.16910346799591, -93.17061901621636,
           -93.16661014505223, -93.23685437388545, -93.24255980272581,
           -93.27349178738028, -93.23563321806229, -93.165863187381, -93.22327937388577),
  lat = c(44.940496165449325, 44.93833990834085, 44.94003346667343,
          44.9279173987216, 44.982074566055694, 44.856749781435035,
          44.958653225483964, 44.981375557267775, 44.93996737805556, 44.97333013850549),
  top = c(FALSE, TRUE, FALSE,
          FALSE, TRUE, FALSE,
          FALSE, FALSE, FALSE, TRUE),
  rank = c(4, 1, 7,
           5, 2, 9,
           10, 6, 8, 3)
  ) %>%
  arrange(rank)

pal <- colorFactor("viridis", 
                     domain = fav_yunyang$top) 

leaflet(data = fav_yunyang) %>%
  addTiles() %>%
  addCircles(lng = ~long, 
             lat = ~lat,
             label = ~place,
             color = ~pal(top),
             weight = 10, 
             opacity = 1) %>% 
  addLegend(pal = pal, 
            values = ~top, 
            opacity = 0.5, 
            title = "Top 3 Favorite",
            position = "bottomright") %>% 
  addPolylines(lng = ~long, 
               lat = ~lat)
```

```{=html}
<div id="htmlwidget-2301b29c354797aab029" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-2301b29c354797aab029">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addCircles","args":[[44.9383399083409,44.9820745660557,44.9733301385055,44.9404961654493,44.9279173987216,44.9813755572678,44.9400334666734,44.9399673780556,44.856749781435,44.958653225484],[-93.1691034679959,-93.2368543738854,-93.2232793738858,-93.175608887381,-93.1666101450522,-93.2356332180623,-93.1706190162164,-93.165863187381,-93.2425598027258,-93.2734917873803],10,null,null,{"interactive":true,"className":"","stroke":true,"color":["#FDE725","#FDE725","#FDE725","#440154","#440154","#440154","#440154","#440154","#440154","#440154"],"weight":10,"opacity":1,"fill":true,"fillColor":["#FDE725","#FDE725","#FDE725","#440154","#440154","#440154","#440154","#440154","#440154","#440154"],"fillOpacity":0.2},null,null,["Macalester College","Kbop Korean Bistro","Legendary Spice","Home","Nelson's Ice Cream","Tasty Pot","Shish","Toppers Pizza","MOA","MIA"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null,null]},{"method":"addLegend","args":[{"colors":["#440154","#FDE725"],"labels":["FALSE","TRUE"],"na_color":null,"na_label":"NA","opacity":0.5,"position":"bottomright","type":"factor","title":"Top 3 Favorite","extra":null,"layerId":null,"className":"info legend","group":null}]},{"method":"addPolylines","args":[[[[{"lng":[-93.1691034679959,-93.2368543738854,-93.2232793738858,-93.175608887381,-93.1666101450522,-93.2356332180623,-93.1706190162164,-93.165863187381,-93.2425598027258,-93.2734917873803],"lat":[44.9383399083409,44.9820745660557,44.9733301385055,44.9404961654493,44.9279173987216,44.9813755572678,44.9400334666734,44.9399673780556,44.856749781435,44.958653225484]}]]],null,null,{"interactive":true,"className":"","stroke":true,"color":"#03F","weight":5,"opacity":0.5,"fill":false,"fillColor":"#03F","fillOpacity":0.2,"smoothFactor":1,"noClip":false},null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]}],"limits":{"lat":[44.856749781435,44.9820745660557],"lng":[-93.2734917873803,-93.165863187381]}},"evals":[],"jsHooks":[]}</script>
```
  
## Revisiting old datasets

This section will revisit some datasets we have used previously and bring in a mapping component. 

### Bicycle-Use Patterns

The data come from Washington, DC and cover the last quarter of 2014.

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`. This code reads in the large dataset right away.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   name = col_character(),
##   lat = col_double(),
##   long = col_double(),
##   nbBikes = col_double(),
##   nbEmptyDocks = col_double()
## )
```

  9. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. This time, plot the points on top of a map. Use any of the mapping tools you'd like.
  

```r
map <- Trips %>% 
  left_join(Stations,
            by = c("sstation" = "name")) %>% 
  filter(!is.na(long) & !is.na(lat)) %>% 
  group_by(sstation) %>% 
  mutate(total = n()) %>% 
  distinct(sstation, .keep_all = TRUE)

pal <- colorNumeric("viridis", 
                     domain = map$total) 

leaflet(data = map) %>%
  addTiles() %>%
  addCircles(lng = ~long, 
             lat = ~lat,
             label = ~sstation,
             opacity = 1,
             color = ~pal(total)) %>% 
  addLegend(pal = pal, 
            values = ~total, 
            opacity = 0.5, 
            title = "total number of departures",
            position = "bottomright")
```

```{=html}
<div id="htmlwidget-d36fb375bee0a73e6ef6" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-d36fb375bee0a73e6ef6">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addCircles","args":[[38.90849,38.89696,38.9308,38.9154,38.898364,38.9086,38.892459,38.934267,38.899408,38.9121,38.886266,38.9059,38.905126,38.90088,38.900283,38.903827,38.802677,38.903407,38.906602,38.900358,38.984691,38.926088,38.8851,38.9172,38.90985,38.929464,38.863833,38.87675,38.889988,38.912659,38.90534,38.9155,38.91554,38.8991,38.89968,38.9008,38.884,38.9066,38.8767,38.936043,38.9375,38.8896,38.9003,38.982456,38.805317,38.9101,38.888251,38.908905,38.922925,38.894758,38.898069,38.9319,38.901539,38.8426,38.894832,38.956432,38.847977,38.912719,38.8946,38.82595,38.900412,38.916787,38.873057,38.899983,38.8637,38.9057,38.921074,38.9176,38.9126,38.897195,38.899972,38.917622,38.912682,38.927095,38.9249,38.917761,38.8743,38.943837,38.930282,38.90093,38.897274,38.8792,38.90304,38.903584,38.889955,38.90276,38.916442,38.905607,38.893241,38.887237,38.920669,38.88412,38.956595,38.897446,38.89593,38.876393,38.92333,38.927872,38.908142,38.947607,38.884961,39.119765,38.873755,38.938736,38.894573,38.879819,38.977933,38.887378,38.87861,38.90572,38.8573,38.899703,38.944551,38.886952,38.900413,38.903582,38.9268,38.889908,38.880151,38.8904,38.90375,38.905707,38.927497,38.932514,38.923583,38.9003,38.88732,38.928644,38.8564,38.8629,38.98128,38.975219,38.934751,38.903819,38.942016,38.910972,38.941154,38.895344,38.918155,38.904742,38.8881,38.889365,38.920387,38.897315,38.888553,38.90706,38.90509,38.992375,38.9346,38.88397,38.933668,38.814577,38.8803,38.803124,38.928743,38.804718,38.903732,38.915417,38.96115,38.805767,38.878433,38.901755,38.920682,38.881185,38.866471,38.862669,38.8963,38.87887,38.9418,38.882788,38.862303,38.897324,38.885801,38.844015,38.805648,38.990249,38.922649,38.887299,38.947774,38.8533,38.955016,38.894851,38.8601,38.882629,38.893028,38.876737,38.923203,38.918809,38.928156,38.820064,38.902,38.894514,38.919077,38.989724,38.871822,38.857866,38.90268,38.88992,38.846222,38.892275,38.88731,38.8997,38.975,38.884,38.898536,38.895914,38.902204,38.897612,38.9022212,38.896923,38.922581,38.881044,38.949662,38.990639,39.085394,38.863897,39.094772,38.880705,38.894941,38.893648,38.897857,38.983627,38.8763,38.897222,39.076331,38.887312,38.880834,38.902061,38.82175,38.899632,38.884616,38.869418,38.866611,38.89054,38.892164,38.889,38.977093,38.891696,38.848441,38.888767,38.895184,39.114688,38.981103,38.894919,38.8815,38.86017,38.850688,38.867262,38.884734,38.986743,38.896015,38.869442,38.961763,38.88412,38.898404,38.84232,38.983525,38.894722,38.865784,38.862478,38.8561,38.810743,38.8444,38.88786,38.894,38.8952,38.820932,38.98954,38.954812,38.85725,38.87501,38.901385,38.90774,38.952369,38.8923,38.86646,39.110314,38.811456,39.084125,38.964992,38.833077,38.852248,38.908473,38.999634,38.867373,38.834108,38.987,38.883921,38.988562,39.084379,38.801111,39.107709,38.856319,38.804378,38.99521,38.843222,38.864702,38.992679,39.095661,38.844711,38.889935,38.837666,38.835213,38.897063,38.854691,39.093783,38.839912,38.860789,39.103091,39.096312,39.000578,38.8692,38.857803,38.848454,38.999388,39.102212,39.099376,39.094103,39.102099,38.89841,38.90381,39.083705,39.123513,38.96497,38.86559,38.878,38.884085,39.120045,38.859254,38.912648,38.908008,38.997653,39.097636,38.898984,38.895929],[-77.063586,-77.00493,-77.0315,-77.0446,-77.027869,-77.0323,-77.046567,-77.057979,-77.015289,-77.0387,-77.022241,-77.0325,-77.056887,-77.048911,-77.029822,-77.053485,-77.063562,-77.043648,-77.038785,-77.012108,-77.094537,-77.036536,-77.0023,-77.0259,-77.034438,-77.027822,-77.080319,-77.02127,-76.995193,-77.017669,-77.046774,-77.0222,-77.03818,-77.0337,-77.041539,-77.047,-76.995397,-77.05152,-77.0178,-77.024649,-77.0328,-76.9769,-76.9882,-77.091991,-77.049883,-77.0444,-77.049426,-77.04478,-77.042581,-76.997114,-77.031823,-77.0388,-77.046564,-77.0502,-76.987633,-77.032947,-77.075104,-77.022155,-77.072305,-77.058541,-77.001949,-77.028139,-76.971015,-76.991383,-77.0633,-77.0056,-77.031887,-77.0321,-77.0135,-76.983575,-76.998347,-77.01597,-77.031681,-76.978924,-77.0222,-77.04062,-77.0057,-77.077078,-77.055599,-77.018677,-76.994749,-76.9953,-77.019027,-77.044789,-77.000349,-77.03863,-77.0682,-77.027137,-77.086045,-77.028226,-77.04368,-77.017445,-77.019815,-77.009888,-77.089006,-77.107735,-77.0352,-77.043358,-77.038359,-77.079382,-77.08777,-77.166093,-77.089233,-77.087171,-77.01994,-77.037413,-77.006472,-77.001955,-77.006004,-77.022264,-77.0511,-77.008911,-77.063896,-76.996806,-76.982872,-77.067786,-77.0322,-76.983326,-77.107673,-77.0889,-77.06269,-77.003041,-76.997194,-76.992889,-77.050046,-77.0429,-76.983569,-76.990955,-77.0492,-77.0528,-77.011336,-77.016855,-77.074647,-77.0284,-77.032652,-77.00495,-77.062036,-77.016106,-77.004746,-77.041606,-77.09308,-77.077294,-77.025672,-77.070993,-77.032429,-77.015231,-76.9941,-77.100104,-76.9955,-77.10783,-76.991016,-77.052808,-76.9862,-77.040363,-77.012457,-77.043363,-76.987211,-77.012289,-77.088659,-77.06072,-77.03023,-77.051084,-76.995876,-77.001828,-77.076131,-76.994637,-77.045,-77.1207,-77.0251,-77.103148,-77.059936,-77.022322,-77.097745,-77.050537,-77.05293,-77.02935,-77.077271,-77.018939,-77.032818,-77.0498,-77.069956,-77.02324,-76.9672,-77.109366,-77.026013,-76.994468,-77.047637,-77.041571,-77.02344,-77.057619,-77.03353,-77.031617,-77.000648,-77.023854,-77.107906,-77.05949,-77.02674,-77.071301,-77.069275,-77.013917,-77.01397,-77.023086,-77.01121,-76.9861,-76.931862,-77.026064,-77.04337,-77.080851,-77.059219,-77.086502,-77.070334,-77.111768,-77.027333,-77.100239,-77.145803,-76.990037,-77.145213,-77.08596,-77.09169,-77.076701,-77.026975,-77.006311,-77.0037,-77.019347,-77.141378,-77.025762,-77.091129,-77.038322,-77.047494,-77.031686,-77.10108,-77.095596,-76.985238,-77.08095,-77.079375,-77.0925,-77.094589,-77.0846,-77.051516,-77.02858,-77.054845,-77.171487,-77.097426,-77.046587,-77.10396,-77.049593,-77.05152,-77.072315,-77.093485,-77.000035,-77.078107,-77.104503,-77.085998,-77.04657,-77.024281,-77.089555,-77.095367,-77.045128,-76.9784,-77.086599,-77.0512,-77.044664,-77.085931,-77.094875,-76.947974,-77.0436,-77.053096,-77.098029,-77.082426,-77.05332,-77.0024,-76.941877,-77.071652,-77.002721,-77.0436,-77.04826,-77.182669,-77.050276,-77.151291,-77.103381,-77.059821,-77.105022,-76.933099,-77.109647,-76.988039,-77.087323,-77.029417,-77.116817,-77.096539,-77.146866,-77.068952,-77.152072,-77.11153,-77.060866,-77.02918,-76.999388,-77.048672,-77.029457,-77.159048,-76.987823,-76.93723,-77.09482,-77.094295,-76.947446,-77.100555,-77.202501,-77.087083,-77.09586,-77.196442,-77.192672,-77.00149,-76.9599,-77.086733,-77.084918,-77.031555,-77.177091,-77.188014,-77.132954,-77.200322,-77.039624,-77.034931,-77.149443,-77.15741,-77.075946,-76.952103,-76.9607,-76.957461,-77.156985,-77.063275,-77.041834,-76.996985,-77.034499,-77.196636,-77.078317,-77.105246],10,null,null,{"interactive":true,"className":"","stroke":true,"color":["#453781","#FDE725","#2C738E","#2B748E","#38588C","#2E6E8E","#46337F","#482979","#453581","#21908D","#3C4F8A","#23A983","#38598C","#34618D","#404688","#355F8D","#471164","#433D84","#34618D","#472E7C","#481668","#2F6B8E","#404688","#404688","#1FA287","#365D8D","#470D60","#481769","#453681","#453581","#31688E","#33628D","#1FA187","#34608D","#3D4E8A","#3E4A89","#26828E","#424186","#3A538B","#3F4788","#463480","#482374","#404688","#450659","#471164","#88D548","#28AE80","#355F8D","#2A788E","#3C4F8A","#3D4D8A","#375B8D","#46337F","#471164","#404688","#481467","#450558","#33638D","#482475","#460B5E","#33638D","#33638D","#450458","#463480","#481567","#2E6F8E","#365D8D","#1F988B","#3D4D8A","#482475","#443B84","#453581","#2C718E","#450559","#481F70","#3A538B","#470F62","#481D6F","#472A7A","#2E6E8E","#423F85","#481E70","#2E6F8E","#46317E","#3E4A89","#31668E","#482677","#2E6E8E","#470F62","#414387","#3D4D8A","#414487","#46085C","#2B768E","#481C6E","#471164","#3C4F8A","#481467","#2D708E","#46307E","#460B5E","#46085C","#450558","#481F70","#414487","#31668E","#470E61","#3E4989","#414287","#31668E","#453681","#472B7A","#482071","#3E4B8A","#482071","#482172","#31688E","#472A7A","#48196B","#481668","#404688","#3D4D8A","#470D60","#471265","#30698E","#453882","#46307E","#460A5D","#471365","#471164","#46075A","#482475","#481D6F","#3D4D8A","#481B6D","#472D7B","#48186A","#472F7D","#482072","#3E4C8A","#470F62","#470D60","#482475","#443983","#1F948C","#453581","#482576","#46075A","#470E61","#470E61","#46095C","#482576","#472F7D","#460B5E","#471365","#471365","#482071","#3E4A89","#450659","#482173","#471365","#443B84","#460C5F","#46327E","#450357","#460B5E","#433E85","#470D60","#482071","#482576","#482071","#433E85","#471365","#460A5D","#460A5D","#46075A","#472F7D","#355F8D","#470E61","#48186A","#481A6C","#414487","#440256","#471265","#414487","#470C5F","#482475","#38598C","#453882","#460C5F","#414287","#3A538B","#481A6C","#46075A","#471063","#470F62","#3B528B","#482072","#450458","#424186","#3F4889","#2E6F8E","#460A5D","#46317E","#440256","#443A83","#46327E","#481466","#443B84","#471265","#481F70","#482475","#470E61","#450559","#440356","#450559","#440256","#470C5F","#470E61","#481467","#424186","#450457","#482576","#3F4788","#440255","#482374","#46065A","#3C508B","#460A5D","#433E85","#471063","#46095C","#450559","#471164","#470C5F","#471366","#46095C","#482475","#481D6F","#32658E","#481568","#450458","#460A5D","#46307E","#460A5D","#471164","#46085C","#46075A","#470F62","#450659","#470D60","#460C5F","#470F62","#463480","#46327E","#46095D","#46085C","#481B6D","#450357","#460A5D","#46095C","#470D60","#450357","#481E6F","#440356","#481C6E","#46085C","#46095D","#46065A","#460A5D","#482374","#440255","#3D4D8A","#450559","#424186","#450457","#450357","#46075A","#440256","#46085B","#46075B","#440356","#440155","#450559","#46075A","#450357","#440356","#460B5E","#46075B","#440256","#450559","#450357","#450457","#450559","#450458","#440256","#450559","#450457","#440256","#440356","#450457","#440256","#440154","#450357","#440154","#440154","#440256","#440155","#440255","#440255","#440255","#440255","#440256","#440256","#450357","#440155","#440154","#440154","#440154","#3E4B8A","#433E85","#440255","#440154","#450659","#440154","#440154","#440154","#440154","#450559","#482475","#46085B","#440155","#440154","#440356","#440155"],"weight":5,"opacity":1,"fill":true,"fillColor":["#453781","#FDE725","#2C738E","#2B748E","#38588C","#2E6E8E","#46337F","#482979","#453581","#21908D","#3C4F8A","#23A983","#38598C","#34618D","#404688","#355F8D","#471164","#433D84","#34618D","#472E7C","#481668","#2F6B8E","#404688","#404688","#1FA287","#365D8D","#470D60","#481769","#453681","#453581","#31688E","#33628D","#1FA187","#34608D","#3D4E8A","#3E4A89","#26828E","#424186","#3A538B","#3F4788","#463480","#482374","#404688","#450659","#471164","#88D548","#28AE80","#355F8D","#2A788E","#3C4F8A","#3D4D8A","#375B8D","#46337F","#471164","#404688","#481467","#450558","#33638D","#482475","#460B5E","#33638D","#33638D","#450458","#463480","#481567","#2E6F8E","#365D8D","#1F988B","#3D4D8A","#482475","#443B84","#453581","#2C718E","#450559","#481F70","#3A538B","#470F62","#481D6F","#472A7A","#2E6E8E","#423F85","#481E70","#2E6F8E","#46317E","#3E4A89","#31668E","#482677","#2E6E8E","#470F62","#414387","#3D4D8A","#414487","#46085C","#2B768E","#481C6E","#471164","#3C4F8A","#481467","#2D708E","#46307E","#460B5E","#46085C","#450558","#481F70","#414487","#31668E","#470E61","#3E4989","#414287","#31668E","#453681","#472B7A","#482071","#3E4B8A","#482071","#482172","#31688E","#472A7A","#48196B","#481668","#404688","#3D4D8A","#470D60","#471265","#30698E","#453882","#46307E","#460A5D","#471365","#471164","#46075A","#482475","#481D6F","#3D4D8A","#481B6D","#472D7B","#48186A","#472F7D","#482072","#3E4C8A","#470F62","#470D60","#482475","#443983","#1F948C","#453581","#482576","#46075A","#470E61","#470E61","#46095C","#482576","#472F7D","#460B5E","#471365","#471365","#482071","#3E4A89","#450659","#482173","#471365","#443B84","#460C5F","#46327E","#450357","#460B5E","#433E85","#470D60","#482071","#482576","#482071","#433E85","#471365","#460A5D","#460A5D","#46075A","#472F7D","#355F8D","#470E61","#48186A","#481A6C","#414487","#440256","#471265","#414487","#470C5F","#482475","#38598C","#453882","#460C5F","#414287","#3A538B","#481A6C","#46075A","#471063","#470F62","#3B528B","#482072","#450458","#424186","#3F4889","#2E6F8E","#460A5D","#46317E","#440256","#443A83","#46327E","#481466","#443B84","#471265","#481F70","#482475","#470E61","#450559","#440356","#450559","#440256","#470C5F","#470E61","#481467","#424186","#450457","#482576","#3F4788","#440255","#482374","#46065A","#3C508B","#460A5D","#433E85","#471063","#46095C","#450559","#471164","#470C5F","#471366","#46095C","#482475","#481D6F","#32658E","#481568","#450458","#460A5D","#46307E","#460A5D","#471164","#46085C","#46075A","#470F62","#450659","#470D60","#460C5F","#470F62","#463480","#46327E","#46095D","#46085C","#481B6D","#450357","#460A5D","#46095C","#470D60","#450357","#481E6F","#440356","#481C6E","#46085C","#46095D","#46065A","#460A5D","#482374","#440255","#3D4D8A","#450559","#424186","#450457","#450357","#46075A","#440256","#46085B","#46075B","#440356","#440155","#450559","#46075A","#450357","#440356","#460B5E","#46075B","#440256","#450559","#450357","#450457","#450559","#450458","#440256","#450559","#450457","#440256","#440356","#450457","#440256","#440154","#450357","#440154","#440154","#440256","#440155","#440255","#440255","#440255","#440255","#440256","#440256","#450357","#440155","#440154","#440154","#440154","#3E4B8A","#433E85","#440255","#440154","#450659","#440154","#440154","#440154","#440154","#450559","#482475","#46085B","#440155","#440154","#440356","#440155"],"fillOpacity":0.2},null,null,["Wisconsin Ave &amp; O St NW","Columbus Circle / Union Station","Park Rd &amp; Holmead Pl NW","20th St &amp; Florida Ave NW","Metro Center / 12th &amp; G St NW","14th &amp; Rhode Island Ave NW","21st St &amp; Constitution Ave NW","Connecticut Ave &amp; Newark St NW / Cleveland Park","3rd &amp; H St NW","17th &amp; Corcoran St NW","L'Enfant Plaza / 7th &amp; C St SW","Thomas Circle","M St &amp; Pennsylvania Ave NW","22nd &amp; I St NW / Foggy Bottom","13th St &amp; New York Ave NW","25th St &amp; Pennsylvania Ave NW","Ballenger Ave &amp; Dulaney St","19th &amp; L St NW","17th &amp; Rhode Island Ave NW","1st &amp; H St NW","Bethesda Metro","16th &amp; Harvard St NW","3rd &amp; D St SE","10th &amp; U St NW","15th &amp; P St NW","11th &amp; Kenyon St NW","Columbia Pike &amp; S Courthouse Rd","6th &amp; Water St SW / SW Waterfront","8th &amp; East Capitol St NE","New Jersey Ave &amp; R St NW","21st &amp; M St NW","7th &amp; T St NW","New Hampshire Ave &amp; T St NW","New York Ave &amp; 15th St NW","18th St &amp; Pennsylvania Ave NW","21st &amp; I St NW","Eastern Market Metro / Pennsylvania Ave &amp; 7th St SE","24th &amp; N St NW","4th &amp; M St SW","Georgia &amp; New Hampshire Ave NW","14th St &amp; Spring Rd NW","19th &amp; East Capitol St SE","13th &amp; H St NE","47th &amp; Elm St","King St &amp; Patrick St","Massachusetts Ave &amp; Dupont Circle NW","Lincoln Memorial","20th &amp; O St NW / Dupont South","Adams Mill &amp; Columbia Rd NW","D St &amp; Maryland Ave NE","14th &amp; G St NW","Lamont &amp; Mt Pleasant NW","21st St &amp; Pennsylvania Ave NW","S Glebe &amp; Potomac Ave","13th &amp; D St NE","14th St &amp; Colorado Ave NW","S Troy St &amp; 26th St S","7th &amp; R St NW / Shaw Library","Rosslyn Metro / Wilson Blvd &amp; Ft Myer Dr","Mount Vernon Ave &amp; E Del Ray Ave","3rd &amp; H St NE","12th &amp; U St NW","Pennsylvania &amp; Minnesota Ave SE","11th &amp; H St NE","S Joyce &amp; Army Navy Dr","1st &amp; M St NE","14th &amp; Belmont St NW","14th &amp; V St NW","Florida Ave &amp; R St NW","15th &amp; F St NE","6th &amp; H St NE","3rd &amp; Elm St NW","14th &amp; R St NW","18th St &amp; Rhode Island Ave NE","Georgia Ave and Fairmont St NW","California St &amp; Florida Ave NW","1st &amp; N St  SE","39th &amp; Veazey St NW","3000 Connecticut Ave NW / National Zoo","5th St &amp; Massachusetts Ave NW","8th &amp; F St NE","8th &amp; Eye St SE / Barracks Row","5th &amp; K St NW","20th &amp; L St NW","4th &amp; East Capitol St NE","17th &amp; K St NW","34th St &amp; Wisconsin Ave NW","11th &amp; M St NW","N Veitch &amp; Key Blvd","USDA / 12th &amp; Independence Ave SW","Columbia Rd &amp; Belmont St NW","4th &amp; E St SW","5th &amp; Kennedy St NW","North Capitol St &amp; F St NW","N Adams St &amp; Lee Hwy","N Quincy St &amp; Glebe Rd","15th &amp; Euclid St  NW","Harvard St &amp; Adams Mill Rd NW","17th St &amp; Massachusetts Ave NW","Tenleytown / Wisconsin Ave &amp; Albemarle St NW","Barton St &amp; 10th St N","Shady Grove Metro West","Arlington Blvd &amp; Fillmore St","Ward Circle / American University","6th St &amp; Indiana Ave NW","Jefferson Memorial","Carroll &amp; Ethan Allen Ave","3rd St &amp; Pennsylvania Ave SE","1st &amp; K St SE","Convention Center / 7th &amp; M St NW","Crystal City Metro / 18th &amp; Bell St","North Capitol St &amp; G Pl NE","Van Ness Metro / UDC","Eastern Market / 7th &amp; North Carolina Ave SE","Bladensburg Rd &amp; Benning Rd NE","34th &amp; Water St NW","14th &amp; Harvard St NW","15th &amp; East Capitol St NE","N Quincy St &amp; Wilson Blvd","Wilson Blvd &amp; Franklin Rd","C &amp; O Canal &amp; Wisconsin Ave NW","M St &amp; Delaware Ave NE","Hamlin &amp; 7th St NE","10th &amp; Monroe St NE","Calvert St &amp; Woodley Pl NW","19th St &amp; Pennsylvania Ave NW","15th St &amp; Massachusetts Ave SE","12th &amp; Irving St NE","20th &amp; Crystal Dr","12th &amp; Army Navy Dr","Philadelphia &amp; Maple Ave","Takoma Metro","Idaho Ave &amp; Newark St NW [on 2nd District patio]","12th &amp; L St NW","14th &amp; Upshur St NW","Eckington Pl &amp; Q St NE","Connecticut Ave &amp; Tilden St NW","4th &amp; D St NW / Judiciary Square","Rhode Island Ave &amp; V St NE","18th &amp; M St NW","Clarendon Blvd &amp; N Fillmore St","Arlington Blvd &amp; N Queen St","10th &amp; Florida Ave NW","Lynn &amp; 19th St North","Jefferson Dr &amp; 14th St SW","New Jersey Ave &amp; N St NW/Dunbar HS","Gallaudet / 8th St &amp; Florida Ave NE","Battery Ln &amp; Trolley Trail","John McCormack Dr &amp; Michigan Ave NE","Central Library / N Quincy St &amp; 10th St N","12th &amp; Newton St NE","Braddock Rd Metro","Potomac &amp; Pennsylvania Ave SE","Prince St &amp; Union St","1st &amp; Washington Hospital Center NW","Market Square / King St &amp; Royal St","Neal St &amp; Trinidad Ave NE","1st &amp; Rhode Island Ave NW","Friendship Blvd &amp; Willard Ave","King St Metro","Hains Point/Buckeye &amp; Ohio Dr SW","New Hampshire Ave &amp; 24th St NW","Rhode Island Ave Metro","3rd &amp; G St SE","Rolfe St &amp; 9th St S","Anacostia Metro","20th &amp; E St NW","George Mason Dr &amp; Wilson Blvd","9th &amp; Upshur St NW","Virginia Square Metro / N Monroe St &amp; 9th St N","Pentagon City Metro / 12th &amp; S Hayes St","7th &amp; F St NW / National Portrait Gallery","Fairfax Dr &amp; Wilson Blvd","Potomac Ave &amp; 35th St S","Commerce St &amp; Fayette St","East West Hwy &amp; Blair Mill Rd","39th &amp; Calvert St NW / Stoddert","Maryland &amp; Independence Ave SW","14th St Heights / 14th &amp; Crittenden St NW","23rd &amp; Crystal Dr","Connecticut &amp; Nebraska Ave NW","8th &amp; D St NW","Good Hope &amp; Naylor Rd SE","N Randolph St &amp; Fairfax Dr","10th St &amp; Constitution Ave NW","Potomac Ave &amp; 8th St SE","Calvert &amp; Biltmore St NW","18th St &amp; Wyoming Ave NW","Columbia Rd &amp; Georgia Ave NW","Mount Vernon Ave &amp; E Nelson Ave","15th &amp; K St NW","14th &amp; D St NW / Ronald Reagan Building","4th &amp; W St NE","Fenton St &amp; Gist Ave","Pershing &amp; N George Mason Dr","Aurora Hills Community Ctr/18th &amp; Hayes St","11th &amp; K St NW","Iwo Jima Memorial/N Meade &amp; 14th St N","28th St S &amp; S Meade St","Constitution Ave &amp; 2nd St NW/DOL","Washington &amp; Independence Ave SW/HHS","8th &amp; H St NW","Carroll &amp; Westmoreland Ave","14th &amp; D St SE","Nannie Helen Burroughs Ave &amp; 49th St NE","10th &amp; E St NW","19th &amp; K St NW","Lee Hwy &amp; N Scott St","Georgetown Harbor / 30th St NW","N Veitch  &amp; 20th St N","36th &amp; Calvert St NW / Glover Park","Ballston Metro / N Stuart &amp; 9th St N","Georgia Ave &amp; Emerson St NW","Norfolk &amp; Rugby Ave","Rockville Metro East","Pleasant St &amp; MLK Ave SE","Frederick Ave &amp; Horners Ln","N Pershing Dr &amp; Wayne St","Lee Hwy &amp; N Cleveland St","Clarendon Blvd &amp; Pierce St","11th &amp; F St NW","Maple &amp; Ritchie Ave","M St &amp; New Jersey Ave SE","5th &amp; F St NW","Fleet St &amp; Ritchie Pkwy","Independence Ave &amp; L'Enfant Plaza SW/DOE","Washington Blvd &amp; 7th St N","17th &amp; K St NW / Farragut Square","Potomac Greens Dr &amp; Slaters Ln","14th St &amp; New York Ave NW","Fairfax Dr &amp; Kenmore St","TJ Cmty Ctr / 2nd St &amp; S Old Glebe Rd","Good Hope Rd &amp; 14th St SE","15th &amp; N Scott St","N Rhodes &amp; 16th St N","Wilson Blvd &amp; N Edgewood St","Offutt Ln &amp; Chevy Chase Dr","Wilson Blvd &amp; N Uhle St","27th &amp; Crystal Dr","Smithsonian / Jefferson Dr &amp; 12th St SW","Kennedy Center","King Farm Blvd &amp; Pleasant Dr","Bethesda Ave &amp; Arlington Rd","US Dept of State / Virginia Ave &amp; 21st St NW","Wilson Blvd &amp; N Oakland St","15th &amp; Crystal Dr","26th &amp; S Clark St","Columbia Pike &amp; S Orme St","Washington Blvd &amp; 10th St N","Washington Adventist U / Flower Ave &amp; Division St","Key Blvd &amp; N Quinn St","Arlington Blvd &amp; S George Mason Dr/NFATC","Friendship Hts Metro/Wisconsin Ave &amp; Wisconsin Cir","Ohio Dr &amp; West Basin Dr SW / MLK &amp; FDR Memorials","MLK Library/9th &amp; G St NW","S Arlington Mill Dr &amp; Campbell Ave","Montgomery &amp; East Ln","20th St &amp; Virginia Ave NW","Anacostia Library","Columbia Pike &amp; S Walter Reed Dr","20th &amp; Bell St","Saint Asaph St &amp; Pendleton  St","S Four Mile Run Dr &amp; S Shirlington Rd","Clarendon Metro / Wilson Blvd &amp; N Highland St","Benning Branch Library","19th &amp; E Street NW","Monroe Ave &amp; Leslie Ave","Cordell &amp; Norfolk Ave","Fessenden St &amp; Wisconsin Ave NW","18th &amp; Eads St.","3rd &amp; Tingey St SE","Nannie Helen Burroughs &amp; Minnesota Ave NE","37th &amp; O St NW / Georgetown University","Fort Totten Metro","19th St &amp; Constitution Ave NW","Long Bridge Park/Long Bridge Dr &amp; 6th St S","King Farm Blvd &amp; Piccard Dr","Henry St &amp; Pendleton St","E Montgomery Ave &amp; Maryland Ave","River Rd &amp; Landy Ln","Mount Vernon Ave &amp; Kennedy St","S George Mason Dr &amp; Four Mile Run","Deanwood Rec Center","Old Georgetown Rd &amp; Southwick St","Good Hope Rd &amp; MLK Ave SE","S Stafford &amp; 34th St S","13th St &amp; Eastern Ave","Glebe Rd &amp; 11th St N","Norfolk Ave &amp; Fairmont St","Rockville Metro West","Eisenhower Ave &amp; Mill Race Ln","Crabbs Branch Way &amp; Calhoun Pl","Columbia Pike &amp; S Dinwiddie St / Arlington Mill Community Center","Duke St &amp; John Carlyle St","Silver Spring Metro/Colesville Rd &amp; Wayne Ave","Alabama &amp; MLK Ave SE","6th &amp; S Ball St","Ripley &amp; Bonifant St","Montgomery College/W Campus Dr &amp; Mannakee St","Congress Heights Metro","Benning Rd &amp; East Capitol St NE / Benning Rd Metro","31st &amp; Woodrow St S","S Abingdon St &amp; 36th St S","Minnesota Ave Metro/DOES","S George Mason Dr &amp; 13th St S","Traville Gateway Dr &amp; Gudelsky Dr","Shirlington Transit Center / S Quincy &amp; Randolph St","S Oakland St &amp; Columbia Pike","Medical Center Dr &amp; Key West Ave","Fallsgrove Blvd &amp; Fallsgrove Dr","Garland Ave &amp; Walden Rd","Branch &amp; Pennsylvania Ave SE","Walter Reed Community Center / Walter Reed Dr &amp; 16th St S","S Kenmore &amp; 24th St S","Georgia Ave &amp; Spring St","Piccard &amp; W Gude Dr","Fallsgrove Dr &amp; W Montgomery Ave","Taft St &amp; E Gude Dr","Broschart &amp; Blackwell Rd","17th &amp; G St NW","15th &amp; L St NW","Monroe St &amp; Monroe Pl","Needwood Rd &amp; Eagles Head Ct","McKinley St &amp; Connecticut Ave NW","Fairfax Village","Randle Circle &amp; Minnesota Ave SE","34th St &amp; Minnesota Ave SE","Crabbs Branch Way &amp; Redland Rd","S Joyce &amp; 16th St S","18th &amp; R St NW","Union Market/6th St &amp; Neal Pl NE","Spring St &amp; Second Ave","Shady Grove Hospital","21st St N &amp; N Pierce St","N Nelson St &amp; Lee Hwy"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null,null]},{"method":"addLegend","args":[{"colors":["#440154 , #472D7B 12.586619629583%, #3B528B 25.1858384780144%, #2C728E 37.7850573264458%, #21918C 50.3842761748772%, #29AF7F 62.9834950233086%, #5FCA61 75.58271387174%, #B0DD2F 88.1819327201713%, #FDE725 "],"labels":["2,000","4,000","6,000","8,000","10,000","12,000","14,000"],"na_color":null,"na_label":"NA","opacity":0.5,"position":"bottomright","type":"numeric","title":"total number of departures","extra":{"p_1":0.12586619629583,"p_n":0.881819327201714},"layerId":null,"className":"info legend","group":null}]}],"limits":{"lat":[38.801111,39.123513],"lng":[-77.202501,-76.931862]}},"evals":[],"jsHooks":[]}</script>
```
  
  10. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? Also plot this on top of a map. I think it will be more clear what the patterns are.
  

```r
pal <- colorFactor("viridis", 
                     domain = map$client) 

leaflet(data = map) %>%
  addTiles() %>%
  addCircles(lng = ~long, 
             lat = ~lat,
             label = ~sstation,
             color = ~pal(client),
             weight = 5, 
             opacity = 1) %>% 
  addLegend(pal = pal, 
            values = ~client, 
            opacity = 0.5, 
            title = "client type",
            position = "bottomright")
```

```{=html}
<div id="htmlwidget-8783342200a62ff5854f" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-8783342200a62ff5854f">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addCircles","args":[[38.90849,38.89696,38.9308,38.9154,38.898364,38.9086,38.892459,38.934267,38.899408,38.9121,38.886266,38.9059,38.905126,38.90088,38.900283,38.903827,38.802677,38.903407,38.906602,38.900358,38.984691,38.926088,38.8851,38.9172,38.90985,38.929464,38.863833,38.87675,38.889988,38.912659,38.90534,38.9155,38.91554,38.8991,38.89968,38.9008,38.884,38.9066,38.8767,38.936043,38.9375,38.8896,38.9003,38.982456,38.805317,38.9101,38.888251,38.908905,38.922925,38.894758,38.898069,38.9319,38.901539,38.8426,38.894832,38.956432,38.847977,38.912719,38.8946,38.82595,38.900412,38.916787,38.873057,38.899983,38.8637,38.9057,38.921074,38.9176,38.9126,38.897195,38.899972,38.917622,38.912682,38.927095,38.9249,38.917761,38.8743,38.943837,38.930282,38.90093,38.897274,38.8792,38.90304,38.903584,38.889955,38.90276,38.916442,38.905607,38.893241,38.887237,38.920669,38.88412,38.956595,38.897446,38.89593,38.876393,38.92333,38.927872,38.908142,38.947607,38.884961,39.119765,38.873755,38.938736,38.894573,38.879819,38.977933,38.887378,38.87861,38.90572,38.8573,38.899703,38.944551,38.886952,38.900413,38.903582,38.9268,38.889908,38.880151,38.8904,38.90375,38.905707,38.927497,38.932514,38.923583,38.9003,38.88732,38.928644,38.8564,38.8629,38.98128,38.975219,38.934751,38.903819,38.942016,38.910972,38.941154,38.895344,38.918155,38.904742,38.8881,38.889365,38.920387,38.897315,38.888553,38.90706,38.90509,38.992375,38.9346,38.88397,38.933668,38.814577,38.8803,38.803124,38.928743,38.804718,38.903732,38.915417,38.96115,38.805767,38.878433,38.901755,38.920682,38.881185,38.866471,38.862669,38.8963,38.87887,38.9418,38.882788,38.862303,38.897324,38.885801,38.844015,38.805648,38.990249,38.922649,38.887299,38.947774,38.8533,38.955016,38.894851,38.8601,38.882629,38.893028,38.876737,38.923203,38.918809,38.928156,38.820064,38.902,38.894514,38.919077,38.989724,38.871822,38.857866,38.90268,38.88992,38.846222,38.892275,38.88731,38.8997,38.975,38.884,38.898536,38.895914,38.902204,38.897612,38.9022212,38.896923,38.922581,38.881044,38.949662,38.990639,39.085394,38.863897,39.094772,38.880705,38.894941,38.893648,38.897857,38.983627,38.8763,38.897222,39.076331,38.887312,38.880834,38.902061,38.82175,38.899632,38.884616,38.869418,38.866611,38.89054,38.892164,38.889,38.977093,38.891696,38.848441,38.888767,38.895184,39.114688,38.981103,38.894919,38.8815,38.86017,38.850688,38.867262,38.884734,38.986743,38.896015,38.869442,38.961763,38.88412,38.898404,38.84232,38.983525,38.894722,38.865784,38.862478,38.8561,38.810743,38.8444,38.88786,38.894,38.8952,38.820932,38.98954,38.954812,38.85725,38.87501,38.901385,38.90774,38.952369,38.8923,38.86646,39.110314,38.811456,39.084125,38.964992,38.833077,38.852248,38.908473,38.999634,38.867373,38.834108,38.987,38.883921,38.988562,39.084379,38.801111,39.107709,38.856319,38.804378,38.99521,38.843222,38.864702,38.992679,39.095661,38.844711,38.889935,38.837666,38.835213,38.897063,38.854691,39.093783,38.839912,38.860789,39.103091,39.096312,39.000578,38.8692,38.857803,38.848454,38.999388,39.102212,39.099376,39.094103,39.102099,38.89841,38.90381,39.083705,39.123513,38.96497,38.86559,38.878,38.884085,39.120045,38.859254,38.912648,38.908008,38.997653,39.097636,38.898984,38.895929],[-77.063586,-77.00493,-77.0315,-77.0446,-77.027869,-77.0323,-77.046567,-77.057979,-77.015289,-77.0387,-77.022241,-77.0325,-77.056887,-77.048911,-77.029822,-77.053485,-77.063562,-77.043648,-77.038785,-77.012108,-77.094537,-77.036536,-77.0023,-77.0259,-77.034438,-77.027822,-77.080319,-77.02127,-76.995193,-77.017669,-77.046774,-77.0222,-77.03818,-77.0337,-77.041539,-77.047,-76.995397,-77.05152,-77.0178,-77.024649,-77.0328,-76.9769,-76.9882,-77.091991,-77.049883,-77.0444,-77.049426,-77.04478,-77.042581,-76.997114,-77.031823,-77.0388,-77.046564,-77.0502,-76.987633,-77.032947,-77.075104,-77.022155,-77.072305,-77.058541,-77.001949,-77.028139,-76.971015,-76.991383,-77.0633,-77.0056,-77.031887,-77.0321,-77.0135,-76.983575,-76.998347,-77.01597,-77.031681,-76.978924,-77.0222,-77.04062,-77.0057,-77.077078,-77.055599,-77.018677,-76.994749,-76.9953,-77.019027,-77.044789,-77.000349,-77.03863,-77.0682,-77.027137,-77.086045,-77.028226,-77.04368,-77.017445,-77.019815,-77.009888,-77.089006,-77.107735,-77.0352,-77.043358,-77.038359,-77.079382,-77.08777,-77.166093,-77.089233,-77.087171,-77.01994,-77.037413,-77.006472,-77.001955,-77.006004,-77.022264,-77.0511,-77.008911,-77.063896,-76.996806,-76.982872,-77.067786,-77.0322,-76.983326,-77.107673,-77.0889,-77.06269,-77.003041,-76.997194,-76.992889,-77.050046,-77.0429,-76.983569,-76.990955,-77.0492,-77.0528,-77.011336,-77.016855,-77.074647,-77.0284,-77.032652,-77.00495,-77.062036,-77.016106,-77.004746,-77.041606,-77.09308,-77.077294,-77.025672,-77.070993,-77.032429,-77.015231,-76.9941,-77.100104,-76.9955,-77.10783,-76.991016,-77.052808,-76.9862,-77.040363,-77.012457,-77.043363,-76.987211,-77.012289,-77.088659,-77.06072,-77.03023,-77.051084,-76.995876,-77.001828,-77.076131,-76.994637,-77.045,-77.1207,-77.0251,-77.103148,-77.059936,-77.022322,-77.097745,-77.050537,-77.05293,-77.02935,-77.077271,-77.018939,-77.032818,-77.0498,-77.069956,-77.02324,-76.9672,-77.109366,-77.026013,-76.994468,-77.047637,-77.041571,-77.02344,-77.057619,-77.03353,-77.031617,-77.000648,-77.023854,-77.107906,-77.05949,-77.02674,-77.071301,-77.069275,-77.013917,-77.01397,-77.023086,-77.01121,-76.9861,-76.931862,-77.026064,-77.04337,-77.080851,-77.059219,-77.086502,-77.070334,-77.111768,-77.027333,-77.100239,-77.145803,-76.990037,-77.145213,-77.08596,-77.09169,-77.076701,-77.026975,-77.006311,-77.0037,-77.019347,-77.141378,-77.025762,-77.091129,-77.038322,-77.047494,-77.031686,-77.10108,-77.095596,-76.985238,-77.08095,-77.079375,-77.0925,-77.094589,-77.0846,-77.051516,-77.02858,-77.054845,-77.171487,-77.097426,-77.046587,-77.10396,-77.049593,-77.05152,-77.072315,-77.093485,-77.000035,-77.078107,-77.104503,-77.085998,-77.04657,-77.024281,-77.089555,-77.095367,-77.045128,-76.9784,-77.086599,-77.0512,-77.044664,-77.085931,-77.094875,-76.947974,-77.0436,-77.053096,-77.098029,-77.082426,-77.05332,-77.0024,-76.941877,-77.071652,-77.002721,-77.0436,-77.04826,-77.182669,-77.050276,-77.151291,-77.103381,-77.059821,-77.105022,-76.933099,-77.109647,-76.988039,-77.087323,-77.029417,-77.116817,-77.096539,-77.146866,-77.068952,-77.152072,-77.11153,-77.060866,-77.02918,-76.999388,-77.048672,-77.029457,-77.159048,-76.987823,-76.93723,-77.09482,-77.094295,-76.947446,-77.100555,-77.202501,-77.087083,-77.09586,-77.196442,-77.192672,-77.00149,-76.9599,-77.086733,-77.084918,-77.031555,-77.177091,-77.188014,-77.132954,-77.200322,-77.039624,-77.034931,-77.149443,-77.15741,-77.075946,-76.952103,-76.9607,-76.957461,-77.156985,-77.063275,-77.041834,-76.996985,-77.034499,-77.196636,-77.078317,-77.105246],10,null,null,{"interactive":true,"className":"","stroke":true,"color":["#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#440154","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#440154","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154"],"weight":5,"opacity":1,"fill":true,"fillColor":["#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#440154","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#440154","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#440154","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#FDE725","#440154"],"fillOpacity":0.2},null,null,["Wisconsin Ave &amp; O St NW","Columbus Circle / Union Station","Park Rd &amp; Holmead Pl NW","20th St &amp; Florida Ave NW","Metro Center / 12th &amp; G St NW","14th &amp; Rhode Island Ave NW","21st St &amp; Constitution Ave NW","Connecticut Ave &amp; Newark St NW / Cleveland Park","3rd &amp; H St NW","17th &amp; Corcoran St NW","L'Enfant Plaza / 7th &amp; C St SW","Thomas Circle","M St &amp; Pennsylvania Ave NW","22nd &amp; I St NW / Foggy Bottom","13th St &amp; New York Ave NW","25th St &amp; Pennsylvania Ave NW","Ballenger Ave &amp; Dulaney St","19th &amp; L St NW","17th &amp; Rhode Island Ave NW","1st &amp; H St NW","Bethesda Metro","16th &amp; Harvard St NW","3rd &amp; D St SE","10th &amp; U St NW","15th &amp; P St NW","11th &amp; Kenyon St NW","Columbia Pike &amp; S Courthouse Rd","6th &amp; Water St SW / SW Waterfront","8th &amp; East Capitol St NE","New Jersey Ave &amp; R St NW","21st &amp; M St NW","7th &amp; T St NW","New Hampshire Ave &amp; T St NW","New York Ave &amp; 15th St NW","18th St &amp; Pennsylvania Ave NW","21st &amp; I St NW","Eastern Market Metro / Pennsylvania Ave &amp; 7th St SE","24th &amp; N St NW","4th &amp; M St SW","Georgia &amp; New Hampshire Ave NW","14th St &amp; Spring Rd NW","19th &amp; East Capitol St SE","13th &amp; H St NE","47th &amp; Elm St","King St &amp; Patrick St","Massachusetts Ave &amp; Dupont Circle NW","Lincoln Memorial","20th &amp; O St NW / Dupont South","Adams Mill &amp; Columbia Rd NW","D St &amp; Maryland Ave NE","14th &amp; G St NW","Lamont &amp; Mt Pleasant NW","21st St &amp; Pennsylvania Ave NW","S Glebe &amp; Potomac Ave","13th &amp; D St NE","14th St &amp; Colorado Ave NW","S Troy St &amp; 26th St S","7th &amp; R St NW / Shaw Library","Rosslyn Metro / Wilson Blvd &amp; Ft Myer Dr","Mount Vernon Ave &amp; E Del Ray Ave","3rd &amp; H St NE","12th &amp; U St NW","Pennsylvania &amp; Minnesota Ave SE","11th &amp; H St NE","S Joyce &amp; Army Navy Dr","1st &amp; M St NE","14th &amp; Belmont St NW","14th &amp; V St NW","Florida Ave &amp; R St NW","15th &amp; F St NE","6th &amp; H St NE","3rd &amp; Elm St NW","14th &amp; R St NW","18th St &amp; Rhode Island Ave NE","Georgia Ave and Fairmont St NW","California St &amp; Florida Ave NW","1st &amp; N St  SE","39th &amp; Veazey St NW","3000 Connecticut Ave NW / National Zoo","5th St &amp; Massachusetts Ave NW","8th &amp; F St NE","8th &amp; Eye St SE / Barracks Row","5th &amp; K St NW","20th &amp; L St NW","4th &amp; East Capitol St NE","17th &amp; K St NW","34th St &amp; Wisconsin Ave NW","11th &amp; M St NW","N Veitch &amp; Key Blvd","USDA / 12th &amp; Independence Ave SW","Columbia Rd &amp; Belmont St NW","4th &amp; E St SW","5th &amp; Kennedy St NW","North Capitol St &amp; F St NW","N Adams St &amp; Lee Hwy","N Quincy St &amp; Glebe Rd","15th &amp; Euclid St  NW","Harvard St &amp; Adams Mill Rd NW","17th St &amp; Massachusetts Ave NW","Tenleytown / Wisconsin Ave &amp; Albemarle St NW","Barton St &amp; 10th St N","Shady Grove Metro West","Arlington Blvd &amp; Fillmore St","Ward Circle / American University","6th St &amp; Indiana Ave NW","Jefferson Memorial","Carroll &amp; Ethan Allen Ave","3rd St &amp; Pennsylvania Ave SE","1st &amp; K St SE","Convention Center / 7th &amp; M St NW","Crystal City Metro / 18th &amp; Bell St","North Capitol St &amp; G Pl NE","Van Ness Metro / UDC","Eastern Market / 7th &amp; North Carolina Ave SE","Bladensburg Rd &amp; Benning Rd NE","34th &amp; Water St NW","14th &amp; Harvard St NW","15th &amp; East Capitol St NE","N Quincy St &amp; Wilson Blvd","Wilson Blvd &amp; Franklin Rd","C &amp; O Canal &amp; Wisconsin Ave NW","M St &amp; Delaware Ave NE","Hamlin &amp; 7th St NE","10th &amp; Monroe St NE","Calvert St &amp; Woodley Pl NW","19th St &amp; Pennsylvania Ave NW","15th St &amp; Massachusetts Ave SE","12th &amp; Irving St NE","20th &amp; Crystal Dr","12th &amp; Army Navy Dr","Philadelphia &amp; Maple Ave","Takoma Metro","Idaho Ave &amp; Newark St NW [on 2nd District patio]","12th &amp; L St NW","14th &amp; Upshur St NW","Eckington Pl &amp; Q St NE","Connecticut Ave &amp; Tilden St NW","4th &amp; D St NW / Judiciary Square","Rhode Island Ave &amp; V St NE","18th &amp; M St NW","Clarendon Blvd &amp; N Fillmore St","Arlington Blvd &amp; N Queen St","10th &amp; Florida Ave NW","Lynn &amp; 19th St North","Jefferson Dr &amp; 14th St SW","New Jersey Ave &amp; N St NW/Dunbar HS","Gallaudet / 8th St &amp; Florida Ave NE","Battery Ln &amp; Trolley Trail","John McCormack Dr &amp; Michigan Ave NE","Central Library / N Quincy St &amp; 10th St N","12th &amp; Newton St NE","Braddock Rd Metro","Potomac &amp; Pennsylvania Ave SE","Prince St &amp; Union St","1st &amp; Washington Hospital Center NW","Market Square / King St &amp; Royal St","Neal St &amp; Trinidad Ave NE","1st &amp; Rhode Island Ave NW","Friendship Blvd &amp; Willard Ave","King St Metro","Hains Point/Buckeye &amp; Ohio Dr SW","New Hampshire Ave &amp; 24th St NW","Rhode Island Ave Metro","3rd &amp; G St SE","Rolfe St &amp; 9th St S","Anacostia Metro","20th &amp; E St NW","George Mason Dr &amp; Wilson Blvd","9th &amp; Upshur St NW","Virginia Square Metro / N Monroe St &amp; 9th St N","Pentagon City Metro / 12th &amp; S Hayes St","7th &amp; F St NW / National Portrait Gallery","Fairfax Dr &amp; Wilson Blvd","Potomac Ave &amp; 35th St S","Commerce St &amp; Fayette St","East West Hwy &amp; Blair Mill Rd","39th &amp; Calvert St NW / Stoddert","Maryland &amp; Independence Ave SW","14th St Heights / 14th &amp; Crittenden St NW","23rd &amp; Crystal Dr","Connecticut &amp; Nebraska Ave NW","8th &amp; D St NW","Good Hope &amp; Naylor Rd SE","N Randolph St &amp; Fairfax Dr","10th St &amp; Constitution Ave NW","Potomac Ave &amp; 8th St SE","Calvert &amp; Biltmore St NW","18th St &amp; Wyoming Ave NW","Columbia Rd &amp; Georgia Ave NW","Mount Vernon Ave &amp; E Nelson Ave","15th &amp; K St NW","14th &amp; D St NW / Ronald Reagan Building","4th &amp; W St NE","Fenton St &amp; Gist Ave","Pershing &amp; N George Mason Dr","Aurora Hills Community Ctr/18th &amp; Hayes St","11th &amp; K St NW","Iwo Jima Memorial/N Meade &amp; 14th St N","28th St S &amp; S Meade St","Constitution Ave &amp; 2nd St NW/DOL","Washington &amp; Independence Ave SW/HHS","8th &amp; H St NW","Carroll &amp; Westmoreland Ave","14th &amp; D St SE","Nannie Helen Burroughs Ave &amp; 49th St NE","10th &amp; E St NW","19th &amp; K St NW","Lee Hwy &amp; N Scott St","Georgetown Harbor / 30th St NW","N Veitch  &amp; 20th St N","36th &amp; Calvert St NW / Glover Park","Ballston Metro / N Stuart &amp; 9th St N","Georgia Ave &amp; Emerson St NW","Norfolk &amp; Rugby Ave","Rockville Metro East","Pleasant St &amp; MLK Ave SE","Frederick Ave &amp; Horners Ln","N Pershing Dr &amp; Wayne St","Lee Hwy &amp; N Cleveland St","Clarendon Blvd &amp; Pierce St","11th &amp; F St NW","Maple &amp; Ritchie Ave","M St &amp; New Jersey Ave SE","5th &amp; F St NW","Fleet St &amp; Ritchie Pkwy","Independence Ave &amp; L'Enfant Plaza SW/DOE","Washington Blvd &amp; 7th St N","17th &amp; K St NW / Farragut Square","Potomac Greens Dr &amp; Slaters Ln","14th St &amp; New York Ave NW","Fairfax Dr &amp; Kenmore St","TJ Cmty Ctr / 2nd St &amp; S Old Glebe Rd","Good Hope Rd &amp; 14th St SE","15th &amp; N Scott St","N Rhodes &amp; 16th St N","Wilson Blvd &amp; N Edgewood St","Offutt Ln &amp; Chevy Chase Dr","Wilson Blvd &amp; N Uhle St","27th &amp; Crystal Dr","Smithsonian / Jefferson Dr &amp; 12th St SW","Kennedy Center","King Farm Blvd &amp; Pleasant Dr","Bethesda Ave &amp; Arlington Rd","US Dept of State / Virginia Ave &amp; 21st St NW","Wilson Blvd &amp; N Oakland St","15th &amp; Crystal Dr","26th &amp; S Clark St","Columbia Pike &amp; S Orme St","Washington Blvd &amp; 10th St N","Washington Adventist U / Flower Ave &amp; Division St","Key Blvd &amp; N Quinn St","Arlington Blvd &amp; S George Mason Dr/NFATC","Friendship Hts Metro/Wisconsin Ave &amp; Wisconsin Cir","Ohio Dr &amp; West Basin Dr SW / MLK &amp; FDR Memorials","MLK Library/9th &amp; G St NW","S Arlington Mill Dr &amp; Campbell Ave","Montgomery &amp; East Ln","20th St &amp; Virginia Ave NW","Anacostia Library","Columbia Pike &amp; S Walter Reed Dr","20th &amp; Bell St","Saint Asaph St &amp; Pendleton  St","S Four Mile Run Dr &amp; S Shirlington Rd","Clarendon Metro / Wilson Blvd &amp; N Highland St","Benning Branch Library","19th &amp; E Street NW","Monroe Ave &amp; Leslie Ave","Cordell &amp; Norfolk Ave","Fessenden St &amp; Wisconsin Ave NW","18th &amp; Eads St.","3rd &amp; Tingey St SE","Nannie Helen Burroughs &amp; Minnesota Ave NE","37th &amp; O St NW / Georgetown University","Fort Totten Metro","19th St &amp; Constitution Ave NW","Long Bridge Park/Long Bridge Dr &amp; 6th St S","King Farm Blvd &amp; Piccard Dr","Henry St &amp; Pendleton St","E Montgomery Ave &amp; Maryland Ave","River Rd &amp; Landy Ln","Mount Vernon Ave &amp; Kennedy St","S George Mason Dr &amp; Four Mile Run","Deanwood Rec Center","Old Georgetown Rd &amp; Southwick St","Good Hope Rd &amp; MLK Ave SE","S Stafford &amp; 34th St S","13th St &amp; Eastern Ave","Glebe Rd &amp; 11th St N","Norfolk Ave &amp; Fairmont St","Rockville Metro West","Eisenhower Ave &amp; Mill Race Ln","Crabbs Branch Way &amp; Calhoun Pl","Columbia Pike &amp; S Dinwiddie St / Arlington Mill Community Center","Duke St &amp; John Carlyle St","Silver Spring Metro/Colesville Rd &amp; Wayne Ave","Alabama &amp; MLK Ave SE","6th &amp; S Ball St","Ripley &amp; Bonifant St","Montgomery College/W Campus Dr &amp; Mannakee St","Congress Heights Metro","Benning Rd &amp; East Capitol St NE / Benning Rd Metro","31st &amp; Woodrow St S","S Abingdon St &amp; 36th St S","Minnesota Ave Metro/DOES","S George Mason Dr &amp; 13th St S","Traville Gateway Dr &amp; Gudelsky Dr","Shirlington Transit Center / S Quincy &amp; Randolph St","S Oakland St &amp; Columbia Pike","Medical Center Dr &amp; Key West Ave","Fallsgrove Blvd &amp; Fallsgrove Dr","Garland Ave &amp; Walden Rd","Branch &amp; Pennsylvania Ave SE","Walter Reed Community Center / Walter Reed Dr &amp; 16th St S","S Kenmore &amp; 24th St S","Georgia Ave &amp; Spring St","Piccard &amp; W Gude Dr","Fallsgrove Dr &amp; W Montgomery Ave","Taft St &amp; E Gude Dr","Broschart &amp; Blackwell Rd","17th &amp; G St NW","15th &amp; L St NW","Monroe St &amp; Monroe Pl","Needwood Rd &amp; Eagles Head Ct","McKinley St &amp; Connecticut Ave NW","Fairfax Village","Randle Circle &amp; Minnesota Ave SE","34th St &amp; Minnesota Ave SE","Crabbs Branch Way &amp; Redland Rd","S Joyce &amp; 16th St S","18th &amp; R St NW","Union Market/6th St &amp; Neal Pl NE","Spring St &amp; Second Ave","Shady Grove Hospital","21st St N &amp; N Pierce St","N Nelson St &amp; Lee Hwy"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null,null]},{"method":"addLegend","args":[{"colors":["#440154","#FDE725"],"labels":["Casual","Registered"],"na_color":null,"na_label":"NA","opacity":0.5,"position":"bottomright","type":"factor","title":"client type","extra":null,"layerId":null,"className":"info legend","group":null}]}],"limits":{"lat":[38.801111,39.123513],"lng":[-77.202501,-76.931862]}},"evals":[],"jsHooks":[]}</script>
```

> ?????
  
### COVID-19 data

The following exercises will use the COVID-19 data from the NYT.

  11. Create a map that colors the states by the most recent cumulative number of COVID-19 cases (remember, these data report cumulative numbers so you don't need to compute that). Describe what you see. What is the problem with this map?


```r
states_map <- map_data("state")

covid19 %>% 
  group_by(state, fips) %>% 
  top_n(n = 1, wt = date) %>% 
  ungroup() %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = str_to_lower(state),
               fill = cases)) +
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map()
```

![](04_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

> ????????????
  
  12. Now add the population of each state to the dataset and color the states by most recent cumulative cases/10,000 people. See the code for doing this with the Starbucks data. You will need to make some modifications. 


```r
covid19 %>% 
  mutate(state = str_to_lower(state)) %>% 
  group_by(state, fips) %>% 
  top_n(n = 1, wt = date) %>% 
  ungroup() %>% 
  left_join(census_pop_est_2018,
            by = "state") %>% 
  mutate(cases_per_10000 = cases / est_pop_2018 * 10000) %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = str_to_lower(state),
               fill = cases_per_10000)) +
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map()
```

![](04_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  
  13. **CHALLENGE** Choose 4 dates spread over the time period of the data and create the same map as in exercise 12 for each of the dates. Display the four graphs together using faceting. What do you notice?


```r
covid19 %>% 
  mutate(state = str_to_lower(state)) %>% 
  filter(date == "2020-03-25" | date == "2020-07-25" | date == "2020-11-25" | date == "2021-03-25") %>% 
  left_join(census_pop_est_2018,
            by = "state") %>% 
  mutate(cases_per_10000 = cases / est_pop_2018 * 10000) %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = str_to_lower(state),
               fill = cases_per_10000)) +
  facet_wrap(~date) +
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map() +
  theme(legend.background = element_blank(), legend.position = "top")
```

![](04_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
> ?????????  
  
## Minneapolis police stops

These exercises use the datasets `MplsStops` and `MplsDemo` from the `carData` library. Search for them in Help to find out more information.

  14. Use the `MplsStops` dataset to find out how many stops there were for each neighborhood and the proportion of stops that were for a suspicious vehicle or person. Sort the results from most to least number of stops. Save this as a dataset called `mpls_suspicious` and display the table.  
 

```r
mpls_suspicious <- MplsStops %>% 
  group_by(neighborhood) %>% 
  mutate(total_stops = n()) %>% 
  filter(problem == "suspicious") %>% 
  mutate(pro_sus = n()/total_stops) %>% 
  distinct(neighborhood, pro_sus, .keep_all = TRUE) %>% 
  arrange(desc(total_stops))
```
  
  15. Use a `leaflet` map and the `MplsStops` dataset to display each of the stops on a map as a small point. Color the points differently depending on whether they were for suspicious vehicle/person or a traffic stop (the `problem` variable). HINTS: use `addCircleMarkers`, set `stroke = FAlSE`, use `colorFactor()` to create a palette.  
  

```r
pal <- colorFactor("viridis", 
                     domain = MplsStops$problem) 

leaflet(data = MplsStops) %>%
  addTiles() %>%
  addCircleMarkers(lng = ~long, 
             lat = ~lat,
             label = ~idNum,
             color = ~pal(problem),
             weight = 0.1, 
             opacity = 0.1,
             radius = 5,
             stroke = FALSE) %>% 
  addLegend(pal = pal, 
            values = ~problem, 
            opacity = 0.5, 
            title = "problem type",
            position = "bottomright")
```

```{=html}
<div id="htmlwidget-11f12e5cdc91cc4a9064" style="width:672px;height:480px;" class="leaflet html-widget"></div>
```
  
  16. Save the folder from moodle called Minneapolis_Neighborhoods into your project/repository folder for this assignment. Make sure the folder is called Minneapolis_Neighborhoods. Use the code below to read in the data and make sure to **delete the `eval=FALSE`**. Although it looks like it only links to the .sph file, you need the entire folder of files to create the `mpls_nbhd` data set. These data contain information about the geometries of the Minneapolis neighborhoods. Using the `mpls_nbhd` dataset as the base file, join the `mpls_suspicious` and `MplsDemo` datasets to it by neighborhood (careful, they are named different things in the different files). Call this new dataset `mpls_all`.


```r
mpls_nbhd <- st_read("Minneapolis_Neighborhoods.shp", quiet = TRUE)

mpls_all <- mpls_nbhd %>% 
  left_join(mpls_suspicious,
            by = c("BDNAME" = "neighborhood")) %>% 
  left_join(MplsDemo,
            by = c("BDNAME" = "neighborhood"))
```

  17. Use `leaflet` to create a map from the `mpls_all` data  that colors the neighborhoods by `prop_suspicious`. Display the neighborhood name as you scroll over it. Describe what you observe in the map.
  

```r
pal <- colorNumeric("viridis", 
                     domain = mpls_all$pro_sus) 

mpls_all2 <- mpls_all %>% 
  distinct(BDNAME, .keep_all = TRUE) %>% 
  filter(!is.na(pro_sus))

leaflet(data = mpls_all) %>%
  addTiles() %>%
  addCircleMarkers(lng = ~long, 
             lat = ~lat,
             label = ~BDNAME,
             color = ~pal(pro_sus)) %>% 
  addLegend(pal = pal, 
            values = ~pro_sus, 
            opacity = 0.5, 
            title = "prop_suspicious",
            position = "bottomright")
```

```
## Warning in validateCoords(lng, lat, funcName): Data contains 1 rows with either
## missing or invalid lat/lon values and will be ignored
```

```{=html}
<div id="htmlwidget-131192676f29f98254cd" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-131192676f29f98254cd">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addCircleMarkers","args":[[44.95014459,44.98045,44.97092,44.96091,44.98074363,45.04759,45.04118133,45.03043868,45.01107619,45.02499559,45.04175181,44.94045427,44.91792713,44.89304316,44.92332,44.91787,44.90881395,44.98875233,44.96661711,44.95777922,44.97366129,45.01235319,45.01043723,44.9915,44.99914804,44.98355662,44.96772584,44.9645834,44.90900519,44.89927,44.94835043,44.91873473,44.98011,44.91335818,44.90895,44.91517938,44.91322327,44.93144733,44.93773,44.9641117,44.95106675,44.94925,44.9474742,null,44.95779751,45.02047,44.89993505,45.01266624,45.02392802,44.92511,44.91708716,45.00494669,45.00524729,45.00594,44.92223954,44.92772137,44.93769702,44.90085942,45.0015864,44.95373,44.9484,44.97086,45.00111564,44.9987,44.99509,44.94750962,44.94567311,44.938634,44.93326015,44.90237056,44.89527691,44.99329,44.94841574,44.96390132,44.9627,44.96091889,44.97246,44.98705365,44.98485046,45.01588734,45.0212088,45.0315,45.03192219,45.03041294,45.019569,44.94836,44.94044075],[-93.26646784,-93.27134,-93.25508,-93.26511,-93.28959927,-93.30931,-93.29786418,-93.2893657,-93.26950762,-93.30835804,-93.31772793,-93.22849787,-93.21689298,-93.27938185,-93.24605,-93.27412,-93.27370052,-93.23458643,-93.24645826,-93.21193648,-93.22790293,-93.31589143,-93.29305945,-93.30572,-93.29181556,-93.3134127,-93.28135089,-93.27658196,-93.21456322,-93.24731,-93.20700558,-93.27795772,-93.28253,-93.25113599,-93.23353,-93.31886431,-93.29339241,-93.26249604,-93.24058,-93.22802405,-93.21783525,-93.29573,-93.29829195,null,-93.24733141,-93.31081,-93.22294772,-93.24912093,-93.23903648,-93.27276,-93.24730493,-93.27152318,-93.25564479,-93.24356,-93.32551557,-93.29082674,-93.28627059,-93.21783647,-93.21321065,-93.25627,-93.32062,-93.32226,-93.24099042,-93.26325,-93.25115,-93.26647891,-93.2626418,-93.24228094,-93.26753888,-93.29083917,-93.31628325,-93.23969,-93.32280644,-93.31189869,-93.29179,-93.2937258,-93.26489,-93.25728617,-93.24510888,-93.29938127,-93.28804287,-93.28757,-93.26577672,-93.23398346,-93.26892853,-93.28135,-93.27791575],10,null,null,{"interactive":true,"className":"","stroke":true,"color":["#22A785","#59C765","#238A8D","#36B878","#20928C","#7BD151","#71CF57","#1F968B","#482677","#24868E","#365D8D","#51C46A","#2FB47C","#2E6E8E","#64CB5F","#6ACD5B","#69CD5B","#443A83","#2EB37C","#3D4D8A","#38588C","#375A8C","#38588C","#306A8E","#33628D","#1F9E89","#4DC36B","#2F6B8E","#9AD93D","#63CB5F","#287C8E","#450457","#33628D","#6ECE58","#D8E219","#33B67A","#3E4C8A","#9CD93C","#A0DA39","#51C569","#48C16E","#355E8D","#1F9E89","#808080","#32B67A","#1F948C","#CEE11D","#472E7C","#3A538B","#5BC863","#D5E21A","#423F85","#48186A","#3F4889","#21918C","#2C738E","#453681","#FDE725","#472F7D","#56C667","#218F8D","#1FA187","#365C8D","#472C7A","#443983","#21A585","#3ABA76","#1F9E89","#7AD151","#228C8D","#9CD93B","#482576","#3E4C8A","#482475","#2A768E","#481466","#68CD5B","#440154","#46337F","#375B8D","#355F8D","#3E4C8A","#32648E","#1F978B","#482979","#355F8D","#355E8D"],"weight":5,"opacity":0.5,"fill":true,"fillColor":["#22A785","#59C765","#238A8D","#36B878","#20928C","#7BD151","#71CF57","#1F968B","#482677","#24868E","#365D8D","#51C46A","#2FB47C","#2E6E8E","#64CB5F","#6ACD5B","#69CD5B","#443A83","#2EB37C","#3D4D8A","#38588C","#375A8C","#38588C","#306A8E","#33628D","#1F9E89","#4DC36B","#2F6B8E","#9AD93D","#63CB5F","#287C8E","#450457","#33628D","#6ECE58","#D8E219","#33B67A","#3E4C8A","#9CD93C","#A0DA39","#51C569","#48C16E","#355E8D","#1F9E89","#808080","#32B67A","#1F948C","#CEE11D","#472E7C","#3A538B","#5BC863","#D5E21A","#423F85","#48186A","#3F4889","#21918C","#2C738E","#453681","#FDE725","#472F7D","#56C667","#218F8D","#1FA187","#365C8D","#472C7A","#443983","#21A585","#3ABA76","#1F9E89","#7AD151","#228C8D","#9CD93B","#482576","#3E4C8A","#482475","#2A768E","#481466","#68CD5B","#440154","#46337F","#375B8D","#355F8D","#3E4C8A","#32648E","#1F978B","#482979","#355F8D","#355E8D"],"fillOpacity":0.2},null,null,null,null,["Phillips West","Downtown West","Downtown East","Ventura Village","Sumner - Glenwood","Shingle Creek","Lind - Bohanon","Webber - Camden","Bottineau","Victory","Humboldt Industrial Area","Howe","Hiawatha","Windom","Ericsson","Field","Page","Como","Cedar Riverside","Prospect Park - East River Road","University of Minnesota","Jordan","Hawthorne","Willard - Hay","Near - North","Harrison","Loring Park","Steven's Square - Loring Heights","Minnehaha","Diamond Lake","Cooper","Tangletown","North Loop","Hale","Keewaydin","Fulton","Lynnhurst","Bancroft","Standish","Seward","Longfellow","Lowry Hill East","ECCO","South Uptown","East Phillips","Cleveland","Wenonah","Holland","Audubon Park","Regina","Northrop","Sheridan","Logan Park","Windom Park","Linden Hills","East Harriet","King Field","Morris Park","Mid - City Industrial","Midtown Phillips","West Calhoun","Bryn - Mawr","Northeast Park","St. Anthony West","St. Anthony East","Central","Powderhorn Park","Corcoran","Bryant","Kenny","Armatage","Beltrami","Cedar - Isles - Dean","Kenwood","Lowry Hill","East Isles","Elliot Park","Nicollet Island - East Bank","Marcy Holmes","Folwell","McKinley","Camden Industrial","Columbia Park","Waite Park","Marshall Terrace","Whittier","Lyndale"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addLegend","args":[{"colors":["#440154 , #470E61 3.49608416768121%, #453882 16.2030054694456%, #365D8D 28.90992677121%, #287C8E 41.6168480729744%, #1E9B8A 54.3237693747388%, #37B878 67.0306906765032%, #78D152 79.7376119782676%, #CCE11E 92.444533280032%, #FDE725 "],"labels":["0.2","0.3","0.4","0.5","0.6","0.7","0.8","0.9"],"na_color":"#808080","na_label":"NA","opacity":0.5,"position":"bottomright","type":"numeric","title":"prop_suspicious","extra":{"p_1":0.0349608416768121,"p_n":0.92444533280032},"layerId":null,"className":"info legend","group":null}]}],"limits":{"lat":[44.89304316,45.04759],"lng":[-93.32551557,-93.20700558]}},"evals":[],"jsHooks":[]}</script>
```
  
> ?????????
  
  18. Use `leaflet` to create a map of your own choosing. Come up with a question you want to try to answer and use the map to help answer that question. Describe what your map shows. 
  

  
## GitHub link

  19. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 04_exercises.Rmd, provide a link to the 04_exercises.md file, which is the one that will be most readable on GitHub.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**